<script>
    import { base } from '$app/paths';
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    import BarHorizontal from '$lib/BarHorizontal.svelte';
    import LineChart from '$lib/LineChart.svelte';
    import {
        computePosition,
        autoPlacement,
        offset,
    } from '@floating-ui/dom';

    let locData = [];
    let commits = [];
    let barData = [];
    let clickedCommits = [];
    let linesByDate = [];

    // Tooltip state
    let hoveredIndex = -1;
    $: hoveredCommit = commits[hoveredIndex] ?? {};
    let commitTooltip;
    let tooltipPosition = {x: 0, y: 0};

    // Brushing state
    let svg;
    let brushSelection = null;
    $: brushedCommits = brushSelection ? commits.filter(isCommitBrushed) : [];
    $: selectedCommits = Array.from(new Set([...clickedCommits, ...brushedCommits]));

    // Layout dimensions
    let width = 928;
    let height = 600;
    const margin = { top: 10, right: 10, bottom: 30, left: 60 };
    let usableArea = {
        top: margin.top,
        right: width - margin.right,
        bottom: height - margin.bottom,
        left: margin.left
    };
    $: usableArea = {
        top: margin.top,
        right: width - margin.right,
        bottom: height - margin.bottom,
        left: margin.left
    };
    $: usableArea.width = usableArea.right - usableArea.left;
    $: usableArea.height = usableArea.bottom - usableArea.top;

    // Scales
    $: xScale = d3.scaleTime()
        .domain(d3.extent(commits, d => d.datetime))
        .range([usableArea.left, usableArea.right])
        .nice();

    $: yScale = d3.scaleLinear()
        .domain([0, 24])
        .range([usableArea.bottom, usableArea.top]);

    // Radius scale using square root to represent area proportionally
    $: rScale = d3.scaleSqrt()
        .domain(d3.extent(commits, d => d.totalLines))
        .range([5, 30]);

    // Axes references
    let xAxis;
    let yAxis;
    let yAxisGridlines;

    // Tooltip interaction handler
    async function dotInteraction(index, evt) {
        let hoveredDot = evt.target;
        if (evt.type === "mouseenter") {
            hoveredIndex = index;
            if (commitTooltip) {
                tooltipPosition = await computePosition(hoveredDot, commitTooltip, {
                    strategy: "fixed",
                    middleware: [
                        offset(5),
                        autoPlacement()
                    ],
                });
            }
        }
        else if (evt.type === "mouseleave") {
            hoveredIndex = -1;
        }
        else if (evt.type === "click") {
            let commit = commits[index];
            if (!clickedCommits.includes(commit)) {
                clickedCommits = [...clickedCommits, commit];
            }
            else {
                clickedCommits = clickedCommits.filter(c => c !== commit);
            }
        }
    }

    function isCommitBrushed(commit) {
        if (!brushSelection) {
            return false;
        }
        const x = xScale(commit.datetime);
        const y = yScale(commit.hourFrac);
        const [[x0, y0], [x1, y1]] = brushSelection;
        return x >= x0 && x <= x1 && y >= y0 && y <= y1;
    }

    function brushed(evt) {
        brushSelection = evt.selection;
    }

    // Setup the brush
    $: {
        if (svg) {
            d3.select(svg).call(
                d3.brush()
                    .extent([[usableArea.left, usableArea.top], [usableArea.right, usableArea.bottom]])
                    .on("start brush end", brushed)
            );
            d3.select(svg).selectAll(".dots, .overlay ~ *").raise();
        }
    }

    onMount(async () => {
        locData = await d3.csv(`${base}/loc.csv`, row => ({
            ...row,
            line: Number(row.line),
            length: Number(row.length),
            depth: Number(row.depth),
            date: new Date(row.date + "T00:00" + row.timezone),
            datetime: new Date(row.datetime)
        }));

        // Step 1.2: Compute commit data
        commits = d3.groups(locData, d => d.commit).map(([commit, lines]) => {
            let first = lines[0];
            let {author, date, time, timezone, datetime} = first;
            let ret = {
                id: commit,
                url: "https://github.com/vis-society/lab-7/commit/" + commit,
                author, date, time, timezone, datetime,
                hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
                totalLines: lines.length,
                lines: lines
            };
            return ret;
        });

        // Sort by descending totalLines so smaller dots are painted last (on top)
        commits = d3.sort(commits, d => -d.totalLines);

        barData = d3.rollups(locData, v => v.length, d => d.type)
            .map(([lang, count]) => ({ label: lang, value: count }));

        // Data wrangling for line chart (Step 2.1)
        // Get count of edits for each date
        const rolled = d3.rollups(
            locData,
            v => v.length,
            d => d3.timeDay.floor(d.date)
        ).map(([date, count]) => ({ date, count }));

        // Get all days between min and max dates
        const [minDate, maxDate] = d3.extent(rolled, d => d.date);
        const allDays = d3.timeDays(minDate, d3.timeDay.offset(maxDate, 1));

        // Fill in missing dates with 0 count
        linesByDate = allDays.map(date => ({
            date,
            count: rolled.find(d => d.date.getTime() === date.getTime())?.count ?? 0
        }));
    });

    // Prepare filtered bar data (Step 5.1)
    $: {
        // Get lines from selected commits or all lines
        let selectedLines = selectedCommits.length > 0 
            ? selectedCommits.flatMap(c => c.lines)
            : locData;
        
        // Count lines by language
        let languageCounts = d3.rollup(selectedLines, v => v.length, d => d.type);
        
        // Get all languages in the codebase
        let allLanguages = Array.from(new Set(locData.map(d => d.type)));
        
        // Create bar chart data with all languages
        barData = allLanguages.map(lang => ({
            label: lang,
            value: languageCounts.get(lang) ?? 0
        }));
    }

    // Reactive statements for axes
    $: {
        if (xAxis) {
            d3.select(xAxis).call(d3.axisBottom(xScale));
        }
        if (yAxis) {
            d3.select(yAxis).call(
                d3.axisLeft(yScale)
                    .tickFormat(d => String(d % 24).padStart(2, "0") + ":00")
            );
        }
        if (yAxisGridlines) {
            d3.select(yAxisGridlines).call(
                d3.axisLeft(yScale)
                    .tickFormat("")
                    .tickSize(-usableArea.width)
            );
        }
    }
</script>

<svelte:head>
    <title>Meta Page</title>
</svelte:head>

<h1>Meta</h1>
<p>Meta page to visualize project data.</p>

<div class="scatterplot">
    <svg {width} {height} viewBox="0 0 {width} {height}" overflow="visible" bind:this={svg} on:mouseleave={() => hoveredIndex = -1} role="img" aria-label="Scatter plot of commits over time">
        <!-- Grid lines -->
        <g class="gridlines" transform="translate({usableArea.left}, 0)" bind:this={yAxisGridlines} />

        <!-- Dots -->
        <g class="dots">
            {#each commits as commit, index}
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <circle
                    role="button"
                    tabindex="0"
                    class:selected={selectedCommits.includes(commit)}
                    cx={xScale(commit.datetime)}
                    cy={yScale(commit.hourFrac)}
                    r={rScale(commit.totalLines)}
                    fill="steelblue"
                    fill-opacity="0.7"
                    on:mouseenter={evt => dotInteraction(index, evt)}
                    on:mouseleave={evt => dotInteraction(index, evt)}
                    on:click={evt => dotInteraction(index, evt)}
                    on:keydown={evt => {
                        if (evt.key === 'Enter' || evt.key === ' ') {
                            dotInteraction(index, { ...evt, type: 'click', target: evt.target });
                        }
                    }}
                />
            {/each}
        </g>

        <!-- Axes -->
        <g class="x-axis" transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
        <g class="y-axis" transform="translate({usableArea.left}, 0)" bind:this={yAxis} />
    </svg>
</div>

<dl class="info tooltip" hidden={hoveredIndex === -1} bind:this={commitTooltip} style="top: {tooltipPosition.y}px; left: {tooltipPosition.x}px">
    <dt>Commit</dt>
    <dd><a href="{hoveredCommit.url}" target="_blank">{hoveredCommit.id}</a></dd>

    <dt>Date</dt>
    <dd>{hoveredCommit.datetime?.toLocaleString("en", {dateStyle: "full"})}</dd>

    <dt>Time</dt>
    <dd>{hoveredCommit.datetime?.toLocaleString("en", {timeStyle: "short"})}</dd>

    <dt>Author</dt>
    <dd>{hoveredCommit.author}</dd>

    <dt>Lines Edited</dt>
    <dd>{hoveredCommit.totalLines}</dd>
</dl>

<BarHorizontal data={barData} title={selectedCommits.length > 0 ? `Lines of Code: ${selectedCommits.length} Selected Commits` : "Website Breakdown"} />

<LineChart data={linesByDate} />

<style>
    .scatterplot {
        margin: 2rem 0;
    }

    .gridlines {
        stroke-opacity: 0.2;
    }

    @keyframes marching-ants {
        to {
            stroke-dashoffset: -8;
        }
    }

    svg :global(.selection) {
        fill-opacity: 10%;
        stroke: black;
        stroke-opacity: 70%;
        stroke-dasharray: 5 3;
        animation: marching-ants 2s linear infinite;
    }

    circle {
        transition: 200ms;
    }

    circle:hover {
        fill: darkgreen;
    }

    circle.selected:hover {
        fill: var(--color-accent, #ff6b6b);
    }

    dl.info {
        display: grid;
        grid-template-columns: auto 1fr;
        gap: 0.5em 1em;
        margin: 0;
        font-size: 0.9em;
        transition-duration: 500ms;
        transition-property: opacity, visibility;
    }

    dl.info dt {
        font-weight: bold;
        color: #666;
    }

    dl.info dd {
        margin: 0;
    }

    .tooltip {
        position: fixed;
        background-color: oklch(100% 0% 0 / 95%);
        border-radius: 0.5rem;
        padding: 0.75rem;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
        backdrop-filter: blur(4px);
        pointer-events: none;
    }

    .tooltip[hidden]:not(:hover, :focus-within) {
        opacity: 0;
        visibility: hidden;
    }

    a {
        color: steelblue;
        text-decoration: none;
    }

    a:hover {
        text-decoration: underline;
    }

    circle.selected {
        fill: var(--color-accent, #ff6b6b);
        fill-opacity: 1;
    }
</style>