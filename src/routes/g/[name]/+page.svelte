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
    <a target="_blank" class="title" href={"https://" + game.domain}>{game.domain}</a>
    <a href="/" class="close"></a>
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
        background-color: #13141e;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
    }

    .title {
        margin: 0;
        padding: 14px;
        font-size: 20px;
        text-decoration: none;
    }

    .close {
        position: absolute;
        right: 8px;
        top: 8px;
        width: 32px;
        height: 32px;
        opacity: 0.3;
    }
    .close:hover {
        opacity: 1;
    }
    .close:before, .close:after {
        position: absolute;
        left: 15px;
        content: ' ';
        height: 33px;
        width: 2px;
        background-color: #fff;
    }
    .close:before {
        transform: rotate(45deg);
    }
    .close:after {
        transform: rotate(-45deg);
    }
</style>
