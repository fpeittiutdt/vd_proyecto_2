<script>
  export let open = false;
  import { slide } from "svelte/transition";
  const handleClick = () => (open = !open);
</script>

<div class="accordion">
  <div class="header">
    <div
      class="text"
      on:click={handleClick}
      style="
    padding: 0 10px;"
    >
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
    margin-bottom: 100px;
    text-align: center;
  }

  div.header {
    display: inline-block;
    width: 30%;
  }

  div.header .text {
    color: white;
    width: auto;
    text-align: center;
  }

  div.details {
    background-color: #cecece;
    padding: 1rem;
    border-radius: 20px;
  }
</style>
