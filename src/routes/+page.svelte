<script>
    import games from '$lib/games.json';
    import categories from '$lib/categories.json';
    import Games from '$lib/Games.svelte';
  import About from '$lib/About.svelte';
</script>

<svelte:head>
    <title>Real Multiplayer Games | Play Now!</title> 
</svelte:head>

<Games selectedCategories={[]}/>

<About/>