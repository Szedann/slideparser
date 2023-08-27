<script lang="ts">
  import { slide } from "svelte/transition";

  interface Slide{
    width: number
    height: number
    url: string
    title: string
  }  
  let slides:Slide[] = []
  let slidesURLInput = ""
  const searchURL = ()=>{
    window.location.search = `url=${slidesURLInput}`
  }
  const getSlides = async (url: string) => {
    const data:Slide[] = await (await fetch(url)).json()
    slides = data
  }
  if(window.location.search){
    const params = new URLSearchParams(window.location.search)
    const url = params.get("url")
    if(url){
      getSlides(url)
    }
  } 
</script>

<main>
  <h1>BC Slide extractor</h1>
  <input type="text" placeholder="presentation url" bind:value={slidesURLInput}>
  <input type="button" value="get slides" on:click={searchURL}>
  {#if slides}
  <div class="slides">

    {#each slides as slide}
    <div>
        <h3>{slide.title}</h3>
        <a href={slide.url} download target="_blank">Download image</a>
        <img src={slide.url} alt="slide" >
      </div>
    {/each}
  </div>
  {/if}
</main>

<style>
  input{
    margin: 1em 0;
  }
  .slides{
    display: flex;
    flex-direction: column;
    max-width: 1000px;
    width: 100%;
    margin: 0 auto;
    gap: 1em;
  }
  .slides div{
    background-color: #0004;
    padding: 1em;
  }
</style>
