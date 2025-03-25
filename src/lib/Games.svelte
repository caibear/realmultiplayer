<script>
    import games from '$lib/games.json';
    import categories from '$lib/categories.json';
    import { goto } from '$app/navigation';
  import { onMount } from 'svelte';

    export let selectedCategories = [];

    let seed = Math.floor(Date.now() / (24 * 60 * 60 * 1000));

    if (false) {
        setInterval(() => {
            seed += 1;
        }, 1000);
    }

    function sort(seed, games) {
        function hashString(string) {
            let hash = 0;
            for (let i = 0; i < string.length; i++) {
                const char = string.charCodeAt(i);
                hash = ((hash << 5) - hash) + char;
                hash = hash & hash;
            }
            return hash;
        }

        function hashGame(game) {
            if (game.name == "yourgame") {
                return Infinity;
            }
            return hashString(`${seed}${game.name}${seed}`);
        }
        return games.sort((a, b) => hashGame(a) - hashGame(b));
    }

    function selectedCategoriesHref(categories, subcategories, selectedCategories, category, selected) {
        let newCategories;
        if (selected) {
            newCategories = selectedCategories.filter(c => c != category);
        } else {
            if (subcategories.includes(category)) {
                newCategories = selectedCategories.filter(c => !subcategories.includes(c));
            }
            newCategories = [...newCategories, category];
        }
        if (newCategories.length == 0) {
            return "/";
        }
        let priority = a => {
            return categories.findIndex(c => c.includes(a));
        };
        newCategories.sort((a, b) => priority(a) - priority(b));
        return `/c/${newCategories.join('-')}`;
    }

    let mounted = false;

    onMount(() => {
        mounted = true;
    })
</script>

<div style="margin: 0.5rem; display: flex; flex-direction: row;">
    <div id="sidebar">
        {#each categories as subcategories, i}
            {#if i != 0}
                <hr>
            {/if}
            {#each subcategories as category}
                {@const selected = selectedCategories.includes(category)}
                <a
                    class="category"
                    class:selected={selected}
                    href={selectedCategoriesHref(
                        categories,
                        subcategories,
                        selectedCategories,
                        category,
                        selected
                    )}
                    data-sveltekit-replacestate
                >{category}</a>
            {/each}
        {/each}
    </div>

    <main>
        {#if mounted}
            {#each sort(seed, games.filter(game => selectedCategories.length == 0 || selectedCategories.every(c => (game.categories || []).includes(c)))) as game}
            {@const yourGame = game.name == "yourgame"}
            <div class="game" class:yourgame={yourGame}>
                <a href={yourGame ? `https://${game.domain}` : `/g/${game.name}`}>
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
        {/if}
    </main>
</div>

<style>
    hr {
        border: 0;
        clear:both;
        display:block;
        width: 96%;
        background-color:#56567c;
        height: 1px;
    }


    main {
        display: flex;
        gap: 0.5rem;
        flex-wrap: wrap;
    }

    #sidebar {
        margin-right: 0.5rem;
        border-radius: 1rem;
        width: 12rem;
        height: 24rem;
        display: flex;
        flex-direction: column;
        background-color: #28283c;
        padding: 0.5rem;
        gap: 0.5rem;
        flex-shrink: 0;
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

    a.category {
        padding: 0.5rem;
        background-color: black;
        border-radius: 0.5rem;
        border: 1px solid transparent;
        cursor: pointer;
    }

    a.category:hover {
        border-color: white;
    }

    a.category.selected {
        background-color: gray;
        cursor: initial;
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
        white-space: nowrap;
    }

    a {
        text-decoration: none;
    }
</style>