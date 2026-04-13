<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
  import Scrolly from "svelte-scrolly";
  import { base } from '$app/paths';
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import Bar from '$lib/Bar.svelte';

  let scrollyProgress = 0;
  let years = projects.map(proj => proj.year);
  let range = Math.max(...years) - Math.min(...years);
  let sorted_projects = projects.sort((a, b) => a.year - b.year);
  let progressPerProject = 100 / sorted_projects.length;
  $: activeProjectIdx = Math.min(sorted_projects.length - 1, Math.floor(scrollyProgress / progressPerProject));

  let rawData = [];
  let wrangled = [];

  onMount(async () => {
    rawData = await d3.json('/lab6_example.json');
    wrangled = d3.rollups(
      rawData,
      v => d3.sum(v, d => d.lines),
      d => d.language
    );
  });

  $: barData = d3.rollups(projects, v => v.length, d => d.year).map(([year, count]) => ({ label: String(year), value: count }));
</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>
<section>
    <h2>Data wrangling result</h2>
    <pre>{JSON.stringify(wrangled, null, 2)}</pre>
</section>

<Bar data={barData} />

<h1>{projects.length} Projects over {range} Years</h1>

<p>Scroll down to see my a timeline of my projects and how they've contributed to my professional and personal life</p>

<div class="scrolly-wrapper">
  <Scrolly bind:progress={scrollyProgress}>
    <section>
      {#each sorted_projects as p}
        <div class="step">
          <div class="step-content">
            <h3>{p.title}</h3>
            <p>{p.story}</p>
          </div>
        </div>
      {/each}
    </section>

    <svelte:fragment slot="viz">
      <div class="project-detail">
        <h3>{sorted_projects[activeProjectIdx].year}</h3>
        <img
          src={sorted_projects[activeProjectIdx].image.startsWith('/')
          ? `${base}${sorted_projects[activeProjectIdx].image}`
          : sorted_projects[activeProjectIdx].image}
          alt="Preview of {sorted_projects[activeProjectIdx].title}"
          style="width: 100%;"
        />
      </div>
    </svelte:fragment>
  </Scrolly>
</div>

<p class="outro">Thanks for scrolling through my project story! Feel free to explore all of the projects at your leisure below.</p>

<h1>Projects I have worked on:</h1>
<h1>{projects.length} Projects</h1>
<div class="projects">
  {#each projects as p}
    <Project data={{
      ...p,
      title: `${p.title} (${p.year})`,
      image: p.image.startsWith('/') ? `${base}${p.image}` : p.image
    }} />
  {/each}
</div>

<style>
  .scrolly-wrapper {
    width: min(1000ch, 60vw);
    position: relative;
    left: 50%;
    transform: translateX(-50%);
  }

  .step {
    min-height: 80vh;
    padding: 2rem;
    display: flex;
    align-items: center;
  }

  .step-content {
    border-left: 3px solid var(--accent-color);
    padding: 1.5rem 2rem;
    max-width: 400px;
  }

  .project-detail {
    padding: 2rem;
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .project-detail h3 {
    font-size: 2rem;
    margin: 0;
  }

  .project-detail img {
    width: 100%;
    max-width: 600px;
    border-radius: 6px;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.15);
  }
</style>