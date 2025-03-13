<script>
    import { Github } from '@lucide/svelte';
    import games from '$lib/games.json';
</script>

<header>
    <span></span>
    <h1>Real Multiplayer Games</h1>
    <a href="https://github.com/caibear/realmultiplayer" target="_blank"><Github/></a>
</header>
<br/>
<main>
  {#each games as game}
    {@const yourGame = game.name == "yourgame"}
    <div class="game" class:yourgame={yourGame}>
        <a href={`https://${game.domain}`} target="_blank">
            {#if yourGame}
                <div class="fakeimg"></div>
                <h3 class="yourgame" style="text-align:center">Your game here!</h3>
            {:else}
                <img src={`/${game.name}_teaser.webp`} alt={game.name}/>
                <h3 class="domain">{game.domain}</h3>
            {/if}
        </a>
    </div>
  {/each}
</main>

<style>
    html, body {
        margin: 0;
    }

    * {
        font-family: sans-serif;
        color: white;
    }

    a {
        text-decoration: none;
    }

    header {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 0.5rem;
    }

    main {
        display: flex;
        gap: 0.5rem;
        flex-wrap: wrap;
    }

    h1 {
        margin: 0;
        letter-spacing: .2rem;
    }

    h3.yourgame {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
    }

    h3.domain {
        opacity: 0;
        position: absolute;
        bottom: 0.5rem;
        left: 0.5rem;
        text-shadow: 0px 0px 5px black, 0px 0px 5px black;
    }

    div.game {
        position: relative;
        width: 20rem;
        height: 20rem;
        border-radius: 1rem;
        overflow: hidden;
        border: 2px solid transparent;
        transition: border-color 0.25s;
    }

    div.yourgame {
        border-color: #999;
    }

    div.game:hover { 
        border-color: white;
    }

    div.game:hover > a > h3.domain {
        opacity: 1;
    }

    img, div.fakeimg {
        position: absolute;
        width: 100%;
        height: 100%;
    }

    h3.domain, h3.yourgame {
        margin: 0;
    }
</style>