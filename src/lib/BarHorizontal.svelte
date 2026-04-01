<script>
    import * as d3 from 'd3';

    export let data = [];
    export let title = "Website Breakdown";

    let width = 400;
    let height = 250;
    let margin = { top: 30, right: 140, bottom: 40, left: 70 };
    let innerWidth  = width  - margin.left - margin.right;
    let innerHeight = height - margin.top  - margin.bottom;

    $: xScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d.value) || 1])
        .range([0, innerWidth]);

    $: yScale = d3.scaleBand()
        .domain(data.map(d => d.label))
        .range([0, innerHeight])
        .padding(0.2);

    $: colorScale = d3.scaleOrdinal(d3.schemeTableau10)
        .domain(data.map(d => d.label));

    $: maxBar = d3.greatest(data, d => d.value);

    let xAxis, yAxis;

    $: if (xAxis && yAxis) {
        d3.select(xAxis).call(
            d3.axisBottom(xScale).ticks(5)
        );
        d3.select(yAxis).call(d3.axisLeft(yScale));
    }
</script>

<div class="container">
    <svg viewBox="0 0 {width} {height}">
        <!-- title -->
        <text
            x={margin.left + innerWidth / 2}
            y={margin.top / 2}
            text-anchor="middle"
            class="chart-title">
            {title}
        </text>

        <!-- x-axis -->
        <g transform="translate({margin.left}, {margin.top + innerHeight})"
           bind:this={xAxis} />

        <!-- y-axis -->
        <g transform="translate({margin.left}, {margin.top})"
           bind:this={yAxis} />

        <!-- bars -->
        <g transform="translate({margin.left}, {margin.top})">
            {#each data as d}
                <rect
                    x={0}
                    y={yScale(d.label)}
                    width={xScale(d.value)}
                    height={yScale.bandwidth()}
                    fill={colorScale(d.label)}
                />
            {/each}

            <!-- annotation -->
            {#if maxBar}
                <rect
                    x={0}
                    y={yScale(maxBar.label)}
                    width={xScale(maxBar.value)}
                    height={yScale.bandwidth()}
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                />
                <text
                    x={xScale(maxBar.value) + 5}
                    y={yScale(maxBar.label) + yScale.bandwidth() / 2}
                    dominant-baseline="middle"
                    text-anchor="start"
                    class="annotation">
                    Most lines
                </text>
            {/if}
        </g>

        <!-- x-axis label -->
        <text
            x={margin.left + innerWidth / 2}
            y={margin.top + innerHeight + margin.bottom - 5}
            text-anchor="middle"
            class="axis-label">
            Lines of Code
        </text>

        <!-- y-axis label -->
        <text
            x={-(margin.top + innerHeight / 2)}
            y={margin.left - 60}
            text-anchor="middle"
            transform="rotate(-90)"
            class="axis-label">
            Language
        </text>
    </svg>

    <ul class="legend">
        {#each data as d}
            <li style="--color: {colorScale(d.label)}">
                <span class="swatch"></span>
                {d.label} <em>({d.value})</em>
            </li>
        {/each}
    </ul>
</div>

<style>
    svg {
        max-width: 100%;
        height: auto;
        overflow: visible;
    }
    .container {
        max-width: 600px;
        margin: 0 auto;
        display: flex;
        align-items: flex-start;
        gap: 1em;
    }
    .legend {
        list-style: none;
        padding: 0;
        margin: 0;
        display: flex;
        flex-direction: column;
        gap: 0.3em;
        font-size: 0.8em;
    }
    .swatch {
        background: var(--color);
        width: 0.8em;
        height: 0.8em;
        border-radius: 2px;
        flex-shrink: 0;
    }
    li {
        display: flex;
        align-items: center;
        gap: 0.4em;
    }
    .chart-title {
        font-size: 0.85em;
        font-weight: bold;
        fill: currentColor;
    }
    .axis-label {
        font-size: 0.7em;
        fill: currentColor;
    }
    .annotation {
        font-size: 0.6em;
        fill: currentColor;
        font-style: italic;
    }
</style>