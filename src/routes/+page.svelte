<script>
    import { Github } from '@lucide/svelte';
    import games from '$lib/games.json';
    import categories from '$lib/categories.json';

    let selectedCategories = [];
</script>

<header>
    <span></span>
    <h1>Real Multiplayer Games</h1>
    <a href="https://github.com/caibear/realmultiplayer" target="_blank"><Github/></a>
</header>
<br/>

<div style="display: flex; flex-direction: row;">
    <div id="sidebar">
        {#each categories as category}
            {@const selected = selectedCategories.includes(category)}
            <!-- svelte-ignore a11y_click_events_have_key_events -->
            <!-- svelte-ignore a11y_no_static_element_interactions -->
            <div class="category" class:selected={selected} on:click={() => {
                if (selected) {
                    selectedCategories = selectedCategories.filter(c => c != category);
                } else {
                    selectedCategories = [...selectedCategories, category];
                }
            }}>{category}</div>
        {/each}
    </div>

    <main>
        {#each games.filter(game => selectedCategories.length == 0 || selectedCategories.every(c => (game.categories || []).includes(c))) as game}
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
</div>


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

    #sidebar {
        margin-right: 0.5rem;
        border-radius: 1rem;
        width: 22rem;
        height: 32rem;
        display: flex;
        flex-direction: column;
        background-color: #28283c;
        z-index: 5;
        padding: 0.5rem;
        gap: 0.5rem;
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
        width: 18rem;
        height: 18rem;
        border-radius: 1rem;
        overflow: hidden;
        border: 2px solid transparent;
        transition: border-color 0.25s;
    }

    div.category {
        padding: 0.5rem;
        background-color: black;
        border-radius: 0.5rem;
        border: 1px solid transparent;
    }

    div.category:hover {
        border-color: white;
    }

    div.category.selected {
        background-color: gray;
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