<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
  import ProjectNarrative from "$lib/ProjectNarrative.svelte";
  import Scrolly from "svelte-scrolly";
  let scrollyProgress = 0;
  let years = projects.map(proj => proj.year);
  let range = Math.max(...years) - Math.min(...years);
  let sorted_projects = projects.sort((a, b) => a.year - b.year);
  let progressPerProject = 100 / sorted_projects.length;
  $: activeProjectIdx = Math.min(sorted_projects.length - 1, Math.floor(scrollyProgress / progressPerProject));
</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>

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
          src={sorted_projects[activeProjectIdx].image}
          alt="Preview of {sorted_projects[activeProjectIdx].title}"
          style="width: 100%;"
        />
      </div>
    </svelte:fragment>
  </Scrolly>
</div>

<p class="outro">Thanks for scrolling through my project story! Feel free to explore all of the projects at your leisure below.</p>

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