<script>
    let distinction = null;
</script>

<div style="margin: 0.5rem; min-height: 25rem;">
    <h2>About</h2>
    <div style="display: flex; flex-direction: column; gap: 1rem;">
        <p style="margin-top: 0.5rem; margin-bottom: 0.5rem;">
            At Real Multiplayer Games, we focus on providing real, multiplayer experiences with no fake players.
            The free browser games we feature are designed to connect you with actual people in live, authentic gameplay.
            With live chat and a community-driven approach, we aim to create a space where you can enjoy genuine, social gaming without the usual gimmicks.
        </p>
        <div id="features">
            {#each ["Browser-based", "Free to Play", "Multiplayer", "Live Chat", "Developer Friendly"] as d}
                <button
                    class="distinction"
                    class:selected={distinction == d}
                    on:click={() => distinction == d ? distinction = null : distinction = d}
                >{d}</button>
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
                    No Commission on Ad Revenue - Include your own ads, such as via
                    <a href="https://adinplay.com/" target="_blank" rel="nofollow">AdinPlay</a>, and
                    keep 100% of your earnings.
                </li>
                <li>
                    Direct Linking - We link directly to your game's website under the cooresponding
                    iframe.
                </li>
                <li>
                    Fair Ranking - Our ranking system randomizes the order of games each UTC day,
                    unlike other platforms with opaque algorithms.
                </li>
                <li>
                    Only Real Multiplayer Games - We only feature real multiplayer games, unlike other
                    platforms that mix fake and real ones.
                </li>
            </ol>
        </div>
    </div>
    <h2>Privacy</h2>
    <p>This is a static site, without cookies, tracking, or data collection.</p>
</div>

<style>
    #features {
        white-space: nowrap;
        width: min-content;
        display: flex;
        flex-direction: row;
        gap: 0.5rem;
    }

    div.distinction {
        display: none;
    }

    div.distinction.selected {
        display: initial;
    }

    button.distinction {
        padding: 0.5rem;
        background-color: black;
        border-radius: 0.5rem;
        border: 1px solid transparent;
        cursor: pointer;
    }

    button.distinction:hover {
        border-color: white;
    }

    button.distinction.selected {
        background-color: gray;
        cursor: initial;
    }
</style>