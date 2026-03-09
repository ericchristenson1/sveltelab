<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import ReadingItem from "$lib/ReadingItem.svelte";
    import readingList from "$lib/reading.json";
    import { onMount } from "svelte";
    import { base } from '$app/paths';

    let githubData = null;
    let loading = true;
    let error = null;




    onMount(async () => {
        try {
            console.log("Page has been mounted!")
            let response = await fetch("https://api.github.com/users/ericchristenson1");
            console.log(response);
            githubData = await response.json();
            console.log(githubData);
        } catch (err) {
            error = err;
        }
        loading = false;
    });
</script>

<h1> Eric Christenson</h1>

<p> This is my personal website, I will upload projects of all different forms here. I am adding more text so I can understand max width</p>

<img class="profile-pic" 
    src="images/Glass1.jpeg"
    alt="Glass vase made with white and blue cane. Next to it is a cup made of the same cane."
    width="300" height="400">

{#if loading}
    <p>Loading...</p>
{:else if error}
    <p>Something went wrong: {error.message}</p>
{:else}
    <div class="github-stats">
    <div class="github-header">
        <h3>GitHub Stats</h3>
    </div>
            <div class="stats-grid">
            <div class="stat-card">
                <span class="stat-value">{githubData.followers}</span>
                <span class="stat-label">Followers</span>
            </div>
            <div class="stat-card">
                <span class="stat-value">{githubData.public_repos}</span>
                <span class="stat-label">Public Repos</span>
            </div>
        </div>
    </div>
    {/if}

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


<style>
.github-stats {
    border: 1px solid #e1e4e8;
    border-radius: 10px;
    padding: 1.25rem 1.5rem;
    max-width: 360px;
    background: #fafafa;
    margin: 1rem auto;
}

.github-header {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin-bottom: 1rem;
    color: #24292f;
}

.github-header h3 {
    margin: 0;
    font-size: 1rem;
    font-weight: 600;
    letter-spacing: 0.02em;
}

.stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.75rem;
}

.stat-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    background: white;
    border: 1px solid #e1e4e8;
    border-radius: 8px;
    padding: 0.75rem 1rem;
    gap: 0.2rem;
}

.stat-value {
    font-size: 1.5rem;
    font-weight: 700;
    color: #24292f;
    line-height: 1;
}

.stat-label {
    font-size: 0.75rem;
    color: #6e7781;
    text-transform: uppercase;
    letter-spacing: 0.05em;
}

.profile-pic {
    border-radius: 8px;
    display: block;
    margin: 1rem auto;
}
</style>