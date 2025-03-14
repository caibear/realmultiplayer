<script>
    import { Github } from '@lucide/svelte';
    import { page } from '$app/state'
	let { children } = $props();
</script>

<header>
    <span></span>
    <a href="/" class:selected={page.url.pathname == "/"}>
        <h1>Real Multiplayer Games</h1>
    </a>
    <a href="https://github.com/caibear/realmultiplayer" target="_blank">
        <Github/>
    </a>
</header>
<br/>

{@render children()}

<style>
    h1 {
        margin: 0;
        letter-spacing: .2rem;
    }

    header {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 0.5rem;
    }

    a {
        text-decoration: none;
    }

    a.selected {
        cursor: default;
    }
</style>