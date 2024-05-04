<script>
  export let open = false;
  import { slide } from "svelte/transition";
  const handleClick = () => (open = !open);
</script>

<div class="accordion">
  <div class="header">
    <div class="text" on:click={handleClick}>
      <slot name="head"></slot>
    </div>
  </div>

  {#if open}
    <div class="details" transition:slide>
      <slot name="details"></slot>
    </div>
  {/if}
</div>

<style>
  div.accordion {
    margin: 1rem 0;
    margin-bottom: 50px;
  }

  div.header {
    display: flex;
    width: 50%;
    background-color: black;
    margin: 0 auto;
  }

  div.header .text {
    flex: 1;
    margin: 10px;
    color: white;
    text-align: center;
  }

  div.details {
    background-color: #cecece;
    padding: 1rem;
  }
</style>
