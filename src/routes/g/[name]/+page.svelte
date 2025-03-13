<script>
    import {page} from '$app/stores';
    import games from '$lib/games.json';
    const name = $page.params.name;
    const game = games.find(g => g.name == name);
</script>

<iframe
    src={`https://${game.domain}`}
    allowfullscreen
    sandbox="allow-forms allow-pointer-lock allow-same-origin allow-scripts" 
    title={game.name}
/>
<footer>
    <h3>{game.domain}</h3>
    <a id="back" href="/">x</a>
</footer>

<style>
    html, body {
        padding: 0;
        overflow: hidden;
    }

    * {
        color: white;
    }

    iframe {
        position: absolute;
        top: 0;
        bottom: 3rem;
        right: 0;
        border: none;
        margin: 0;
        padding: 0;
        overflow: hidden;
        width: 100%;
        height: calc(100% - 3rem);
    }

    footer {
        position: absolute;
        left: 0;
        right: 0;
        bottom: 0;
        height: 3rem;
        max-height: 3rem;
        background-color: black;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    h3 {
        margin: 0;
    }

    #back {
        padding: 0.5rem;
        border-radius: 0.5rem;
        background-color: gray;
    }
</style>
