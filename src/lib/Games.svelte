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

<div class="games">
    <div id="sidebar">
        {#each categories as subcategories, i}
            {#if i != 0}
                <hr>
            {/if}
            <div class="subcategory">
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
            </div>
        {/each}
    </div>

    <main class:mounted>
        {#each sort(seed, games.filter(game => selectedCategories.length == 0 || selectedCategories.every(c => (game.categories || []).includes(c)))) as game}
            {#key game.name}    
                {@const yourGame = game.name == "yourgame"}
                <div class="game" class:yourgame={yourGame}>
                    <a href={yourGame ? `https://${game.domain}` : `/g/${game.name}`}>
                        {#if yourGame}
                            <div class="fakeimg">
                                <h3 class="yourgame" style="text-align:center">Your game here!</h3>
                            </div>
                        {:else}
                            <img src={`/${game.teaser}`} alt={game.name}/>
                            <h3 class="domain">{game.name}</h3>
                        {/if}
                    </a>
                </div>
            {/key}
        {/each}
    </main>
</div>

<style>
    .games{
        margin: 0 0.5rem;
        display: flex;
    }

    hr {
        border: 0;
        clear:both;
        display:block;
        width: 96%;
        background-color:#56567c;
        height: 1px;
    }

    main {
        gap: 0.5rem;
        filter: contrast(0) brightness(0.3);
    }

    main.mounted {
        filter: none;
    }

    #sidebar{
        display: flex;
        flex-shrink: 0;
        background-color: #28283c;
        border-radius: 1rem;
        padding: 0.5rem;
        border: none;
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
        border-radius: 1rem;
        overflow: hidden;
        border: 2px solid transparent;
        transition: border-color 0.25s;
    }

    .subcategory {
        display: flex;
        flex-shrink: 0;
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
        width: 100%;
        height: 100%;
    }

    h3.domain {
        margin: 0;
        white-space: nowrap;
    }

    a {
        text-decoration: none;
    }

    /* 52.5rem = width until less than two columns in main */

    @media screen and (min-width: 52.5rem) {
        .games{
            flex-direction: row;
        }

        main {
            display: flex;
            flex-wrap: wrap;
        }

        #sidebar {
            margin-right: 0.5rem;
            width: 12rem;
            min-height: 24rem;
            height: min-content;
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .subcategory {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        div.game {
            width: 18rem;
            height: 18rem;
        }

        img, .fakeimg {
            position: absolute;
        }

        h3.yourgame {
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            margin: 0;
            white-space: nowrap;
        }
    }

    @media screen and (max-width: 52.5rem) {
        .games {
            flex-direction: column;
            gap: 1rem;
        }

        main {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
        }

        #sidebar {
            flex-direction: column;
            user-select: none;
            gap: .25rem;
        }

        #sidebar button {
            cursor: pointer;
        }

        .subcategory {
            display: flex;
            flex-direction: row;
            flex-wrap: nowrap;
            gap: 0.25rem;
            overflow-x: auto;
            overflow-y: hidden;
        }

        div.game {
            width: 100%;
            height: 100%;
        }

        img, .fakeimg {
            position: relative;
        }

        .fakeimg {
            display: flex;
            flex-direction: row;
            align-items: center;
            aspect-ratio: 1/1;
        }

        h3.yourgame {
            width: 100%;
            padding: .5rem;
        }
    }
</style>