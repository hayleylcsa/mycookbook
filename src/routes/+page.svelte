<script>

    import { onMount } from 'svelte';
    import myPhoto from '$lib/assets/my-photo.png';

    let recipes = $state([]);
    let tab = $state('recipes');
    let selected = $state(null);

    let name = $state('');
    let ingredients = $state('');
    let instructions = $state('');

    onMount(() => {recipes = JSON.parse(localStorage.getItem('recipes') ?? '[]');});

    function store(){
        localStorage.setItem('recipes', JSON.stringify(recipes));
    }

    // add a recipe
    function addRecipe() {
        if (!name.trim()) return;
        recipes.push({name, ingredients, instructions});
        store();
        name = ingredients = instructions = '';
        showList();
    }
    
    function showList(){
        selected = null;
        tab = 'recipes';
    }

    function viewRecipe(recipe) {
        selected = recipe;
    }

    function deleteRecipe(recipe) {
        recipes = recipes.filter((r) => r !== recipe);
        store();
        showList();
    }
</script>

<hgroup>
    <img src={myPhoto} alt="Description" class="pix-art"/>
    <h1>sweet surprises</h1>
    <p class="subheader">recipes for desserts and sweet drinks!</p>
    
</hgroup>
<div class="scalloped-edge"></div>

<main>
    <nav>
        <button class:on={tab === 'recipes'} onclick={showList}>recipes</button>
        <button class:on={tab === 'new'} onclick={() => (tab = 'new')}>new recipe</button>
    </nav>


    <div class="paper">
        {#if tab === 'new'}
        <!-- Add new recipe -->
        <input placeholder="recipe name..." bind:value={name} />
        <textarea rows="4" placeholder="ingredients..." bind:value={ingredients}></textarea>
        <textarea rows="4" placeholder="instructions..." bind:value={instructions}></textarea>
        <button onclick={addRecipe}>save recipe</button>

        {:else if selected}
        <!-- view recipe info-->
         <h2>{selected.name}</h2>
         <h3> ♡ ingredients </h3>
         <p>{selected.ingredients}</p>
         <h3> ♡ instructions</h3>
         <p>{selected.instructions}</p>
         <button onclick={showList}>back</button> 
         
         <button onclick={() => deleteRecipe(selected)}>delete</button>

        {:else}
        {#each recipes as recipe}
            <button class="link" onclick={() => viewRecipe(recipe)}>{recipe.name}</button>
        {:else}
            <p> No recipes yet. Tap "new recipes" to add! </p>
        {/each}  
     {/if}
    </div>
</main>



<style>

@import url('https://fonts.googleapis.com/css2?family=Geist+Pixel&display=swap');

     :global(body) {
        margin: 0;
        color: #3e3558;
        font-family: 'Geist Pixel', sans-serif;
        /* gingham background */
           background-color: #f7f5fb;
        background-image:
        repeating-linear-gradient(90deg, #dcddf0 0px 16px, transparent 16px 32px),
        repeating-linear-gradient(0deg, #ecedf8 0px 16px, transparent 16px 32px);
    
    }
    hgroup{
        background:  #b9bceb;
        text-align: center;
        padding: 30px 44px 35px;
        line-height: 15px;
    }
    .pix-art{
        
        width: 150px;
        margin-top: 10px;
        margin-bottom: 5px;

    }
     .scalloped-edge{
        
        height: 30px;
        background: radial-gradient(circle at 50% 0,  #b9bceb 29.5px, transparent 30px);
        background-size: 60px 30px;
        margin-top: -1px;
        filter: drop-shadow(0 4px 3px rgba(90, 46, 46, .18));
        position: relative;
        z-index: 1;
    
    }
    main{
        max-width: 600px;
        margin: 55px auto;
        padding: 0 16px;
    }
    nav{
        display: flex;
        gap: 12px;
        margin-bottom: 9px;
        position: relative;
    }
    button{
        font: inherit;
        font-weight: bold;
        background: #b9bceb;
        border: 0;
        border-radius: 6px;
        padding: 4px 12px;
        cursor: pointer;
        color: #3e3558;
    }
    .link{
        display: block;
        background: none;
        padding: 0;
        font-weight: normal;
        line-height: 32px;
        text-align: left;
    }
    .link:hover{
        text-decoration: underline;
    }
   .on{
    background: #fff;
   }

   .paper{
    background: #f7f5fb repeating-linear-gradient(transparent 0 31px, #3e3558 31px 32px);
    border-left: 5px solid #3e3558;
    border-radius: 16px;
    padding: 0 20px 32px 30px;
    line-height: 32px;
    min-height: 300px;
     filter: drop-shadow(0 4px 3px rgba(90, 46, 46, .18));  
   }
   h2, h3, p {
    margin: 0;
    font-size: 1rem;
   }
   p {
    white-space: pre-line;
   }
   input, textarea {
        display: block;
        width: 100%;
        font: inherit;
        line-height: 32px;
        background: none;
        border: 0;
        padding: 0;
        resize: none;
   }
</style>