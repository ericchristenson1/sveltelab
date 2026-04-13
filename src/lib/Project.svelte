<script>
    export let data = {};
    import * as d3 from 'd3';
</script>
<article>
            <h2>{data.title}</h2>
            <img src={data.image} alt="">
            <p>{data.description}</p>
            {#if data.url}
                <a href={data.url} target="_blank" rel="noopener noreferrer" class="project-link">View Project →</a>
            {/if}
</article>


<style>
    article {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 1rem;
        padding: 1rem;
        border-radius: 0.5rem;
        background-color: #f0f0f0;
    }
    img {
        max-width: 100%;
        height: auto;
        border-radius: 0.5rem;
    }
    .project-link {
        text-decoration: none;
        color: #0066cc;
        font-weight: 500;
        padding: 0.5rem 1rem;
        border: 1px solid #0066cc;
        border-radius: 0.25rem;
        transition: all 0.2s ease;
    }
    .project-link:hover {
        background-color: #0066cc;
        color: white;
    }
</style>