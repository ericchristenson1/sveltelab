<svelte:head>
  <title>Contact me</title>
</svelte:head>
    <nav class="nav">
        <a href=".">Home</a>
        <a href="projects">Projects</a>
        <a class="currentsite" href="contact">Contact</a>
        <a href="resume">Resume</a>
        <a href="https://github.com/ericchristenson1" target="_blank">Github</a>
    </nav>
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
