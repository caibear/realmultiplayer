<script>
    import { Github } from '@lucide/svelte';
    import games from '$lib/games.json';
    import categories from '$lib/categories.json';
    import Games from '$lib/Games.svelte';
    import {page} from '$app/state';
    import { afterNavigate, onNavigate } from '$app/navigation';
  import About from '$lib/About.svelte';

    let selectedCategories = page.params.categories.split('-');

    afterNavigate(() => {
        selectedCategories = page.params.categories.split('-');
    });
</script>

<svelte:head>
    <title>Play {selectedCategories.join(" ")} games on Real Multiplayer Games</title> 
    <meta name="description" content="Play free {selectedCategories.join(" ")} games in your browser">
</svelte:head>

<Games selectedCategories={selectedCategories}/>

<About/>