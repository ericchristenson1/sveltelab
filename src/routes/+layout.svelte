<script>
import { base } from "$app/paths";
import { page } from "$app/stores";
let pages = [
  {url: "/", title: "About"},
  {url: "/projects", title: "Projects"},
  {url: "/resume", title: "Resume"},
  {url: "/contact", title: "Contact"},
  {url: "https://github.com/ericchristenson1", title: "Github"},
  // add other pages here
];
let colorScheme = "light dark";
let root = globalThis.document?.documentElement;
$: root?.style.setProperty("color-scheme", colorScheme);
let localStorage = globalThis.localStorage ?? {};
if (localStorage.colorScheme) { // if localStorage has a colorScheme property
  colorScheme = localStorage.colorScheme; // override the default colorScheme
}

localStorage.colorScheme = colorScheme;


</script>

<label class="color-scheme-switch">
    Theme:
    <select bind:value={ colorScheme }>
        <option value="light dark">Automatic</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
    </select>
</label>
<nav class="nav">
  {#each pages as p}
    {@const isExternal = p.url.startsWith("http")}

    <a
      href={isExternal ? p.url : base + p.url}
      target={isExternal ? "_blank" : null}
      rel={isExternal ? "noopener noreferrer" : null}
      class:currentsite={
        !isExternal &&
        (p.url === "/"
          ? $page.url.pathname === (base + "/")
          : $page.url.pathname.startsWith(base + p.url))
      }
    >
      {p.title}
    </a>
  {/each}
</nav>

<style>
    .color-scheme-switch {
    position: absolute;
    top: 1rem;
    right: 1rem;
    padding: 1em;
    display: inline-flex;
    gap: 4px;
    font-size: 80%;
    }
    /* Style for the navigation bar */
.nav {
	display: flex;
	color: rgb(63, 123, 63);
	border-bottom-style: solid;

}

.nav a {
	display: inline-block;
	text-decoration: none;
	color: rgb(63, 123, 63);
	text-align: center;
	padding: 1em;
	margin-bottom: auto;
	margin-inline: auto;

}


.nav a.currentsite {
	font-weight: bold;
	border-bottom-width: 0.5em;
	border-bottom-style: solid;
	padding-bottom: 0.5em;
}


.nav a:hover {
	border-bottom-color: var(--color-accent);
	background-color: oklch(from var(--color-accent) 95% 5% h);
}
</style>

<slot />