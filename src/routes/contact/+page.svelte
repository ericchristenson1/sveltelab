<svelte:head>
  <title>Contact me</title>
</svelte:head>

    <h1>Contact Information</h1>
    <form class="contact" action="mailto:echristenson1@outlook.com" method="get">
        <label for="email">Email:</label>
        <input name="email" type="email">
        <label for="subject">Subject:</label>
        <input name="subject">
        <label for="body">Body:</label>
        <textarea name="body"></textarea>
        <label for="button"></label> 
        <button>Send</button>
    </form>
<style>
    .contact { /* I dont know if this is right */
	display: grid;
	grid-template-columns: auto 1fr;
	grid-column: 1 / -1;
	gap: 1em;
}
</style>