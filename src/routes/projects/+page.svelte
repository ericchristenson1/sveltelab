<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
</script>
<svelte:head>
  <title>Projects</title>
</svelte:head>

    <nav class="nav">
        <a href=".">Home</a>
        <a class="currentsite" href="projects">Projects</a>
        <a href="contact">Contact</a>
        <a href="resume">Resume</a>
        <a href="https://github.com/ericchristenson1" target="_blank">Github</a>
    </nav>
    <h1>Projects I have worked on:</h1>
    <!-- Navigation menu: -->
    <h1>{projects.length} Projects</h1>
    <div class="projects">
        {#each projects as p}
        <Project data={p} />
        {/each}
    </div>
