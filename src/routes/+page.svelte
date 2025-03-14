<script>
    import { Github, Hash } from '@lucide/svelte';
    import games from '$lib/games.json';
    import categories from '$lib/categories.json';

    let selectedCategories = [];

    let distinction = null;

    let seed = Date.now() / (24 * 60 * 60 * 1000);

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
</script>

<header>
    <span></span>
    <h1>Real Multiplayer Games</h1>
    <a href="https://github.com/caibear/realmultiplayer" target="_blank"><Github/></a>
</header>
<br/>

<div style="display: flex; flex-direction: row;">
    <div id="sidebar">
        {#each categories as subcategories, i}
            {#if i != 0}
                <hr>
            {/if}
            {#each subcategories as category}
                {@const selected = selectedCategories.includes(category)}
                <!-- svelte-ignore a11y_click_events_have_key_events -->
                <!-- svelte-ignore a11y_no_static_element_interactions -->
                <button class="category" class:selected={selected} on:click={() => {
                    if (selected) {
                        selectedCategories = selectedCategories.filter(c => c != category);
                    } else {
                        if (subcategories.includes(category)) {
                            selectedCategories = selectedCategories.filter(c => !subcategories.includes(c));
                        }
                        selectedCategories = [...selectedCategories, category];
                    }
                }}>{category}</button>
            {/each}

        {/each}
    </div>

    <main>
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
    </main>
</div>
<h2>About</h2>
<div style="display: flex; flex-direction: column; gap: 1rem;">
    <p style="margin-top: 0.5rem; margin-bottom: 0.5rem;">
        At Real Multiplayer Games, we focus on providing real, multiplayer experiences with no fake players.
        The free browser games we feature are designed to connect you with actual people in live, authentic gameplay.
        With live chat and a community-driven approach, we aim to create a space where you can enjoy genuine, social gaming without the usual gimmicks.
    </p>
    <div id="features">
        {#each ["Browser-based", "Free to Play", "Multiplayer", "Live Chat", "Developer Friendly"] as d}
            <button class="distinction" class:selected={distinction == d} on:click={() => distinction = d}>{d}</button>
        {/each}
    </div>
    <div class="distinction" class:selected={distinction == "Browser-based"}>
        Browser-based games offer key advantages like easy access-no downloads or installs required
        and the ability to play on almost any device with internet access. They're often free,
        cross-platform, and don't demand high-end hardware. With built-in social features and quick,
        casual gameplay, they're perfect for players looking for a simple, accessible gaming experience
        without the complexity of desktop or mobile games.
    </div>
    <div class="distinction" class:selected={distinction == "Free to Play"}>
        Free-to-play games allow you to try them without any upfront cost, making them accessible to
        a wider range of players.
    </div>
    <div class="distinction" class:selected={distinction == "Multiplayer"}>
        Authentic multiplayer games are crucial because they create real, engaging interactions with
        other players, making the experience more dynamic and unpredictable. Playing against actual
        people, rather than just bots, brings a sense of challenge, competition, and cooperation that
        enhances the fun. Authentic multiplayer also fosters a genuine sense of community, where you
        can connect, communicate, and share experiences with others in real time, making the game more
        enjoyable and rewarding.
    </div>
    <div class="distinction" class:selected={distinction == "Live Chat"}>
        Live chat is key in authentic multiplayer games as it proves you're playing with real people,
        not bots. It also enhances the experience by allowing real-time communication, making the game
        more social, immersive, and connected.
    </div>
    <div class="distinction" class:selected={distinction == "Developer Friendly"}>
        <ol>
            <li>
                No Commission on Ad Revenue - We don't take a cut of your ad earnings, so you keep it
                all.
            </li>
            <li>
                Direct Linking - You can link directly to your own site, giving you full control over
                your presence.
            </li>
            <li>
                Fair Ranking - Our ranking system randomizes the order of games each UTC day,
                unlike competitors with opaque algorithms.
            </li>
            <li>
                Only Real Multiplayer Games - We only feature real multiplayer games, unlike other
                platforms that mix fake and real ones.
            </li>
        </ol>
    </div>
</div>

<style>
    html, body {
        margin: 0;
    }

    a {
        text-decoration: none;
    }

    hr {
        border: 0;
        clear:both;
        display:block;
        width: 96%;
        background-color:#56567c;
        height: 1px;
    }

    header {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        padding: 0.5rem;
    }

    #features {
        white-space: nowrap;
        width: min-content;
        display: flex;
        flex-direction: row;
        gap: 0.5rem;
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

    button.category, button.distinction {
        padding: 0.5rem;
        background-color: black;
        border-radius: 0.5rem;
        border: 1px solid transparent;
        cursor: pointer;
    }

    button.category:hover, button.distinction:hover {
        border-color: white;
    }

    button.category.selected, button.distinction.selected {
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

    div.distinction {
        display: none;
    }

    div.distinction.selected {
        display: initial;
    }
</style>