<script>
    export let data = {};
</script>
<article>
            <h2>{data.title}</h2>
            <h3>{data.author}</h3>
            <img src={data.image} alt="">
            <p>{data.opinion}</p>
</article>


<style>
article {
    display: grid;

    /* Title / Author / Image / Review */
    grid-template-rows: 
        minmax(3em, auto)   /* title */
        minmax(2em, auto)   /* author */
        1fr                 /* image */
        minmax(2em, auto);  /* review */

    gap: 0.75rem;
    padding: 1.5rem;

    background: #f3f3f3;
    border-radius: 1rem;
    }
    img {
        max-width: 100%;
        height: auto;
        border-radius: 0.5rem;
    }
</style>