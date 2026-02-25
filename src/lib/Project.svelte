<script>
    export let data = {};
</script>
<article>
            <h2>{data.title}</h2>
            <img src={data.image} alt="">
            <p>{data.description}</p>
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
</style>