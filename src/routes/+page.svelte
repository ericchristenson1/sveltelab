<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
  import ReadingItem from "$lib/ReadingItem.svelte";
  import readingList from "$lib/reading.json";
</script>

<h1> Eric Christenson</h1>

<p> This is my personal website, I will upload projects of all different forms here. I am adding more text so I can understand max width</p>

<img src="images/Glass1.jpeg"
    alt="Glass vase made with white and blue cane. Next to it is a cup made of the same cane."
    width="300" height="400">

    <h3>Recent Projects</h3>
    <div class="projects highlights">
        {#each projects.slice(0, 3) as p}
        <Project data={p} />
        {/each}
    </div>

<h3>Reading List</h3>
<div class="projects highlights">
    {#each readingList.slice(0, 3) as r}
    <ReadingItem data={r} />
    {/each}
</div>