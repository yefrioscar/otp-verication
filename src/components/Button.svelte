<script>
  import { fly } from "svelte/transition";

    export let isLoading = false;
    export let text;
</script>

<style>
  Button {
    width: 100%;
    height: 40px;
    border: none;

    background-color: blueviolet;
    color: white;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    transition: all 150ms ease-in-out;
    outline: none;
    margin-bottom: 0.5rem;
  }

  Button:hover {
    background-color: rgb(105, 6, 197);
  }

  Button:active {
    transform: scale(1.05);
  }
</style>


<button on:click>
    {#if isLoading}
         Loading...
    {:else}
        { text }
    {/if}
</button>