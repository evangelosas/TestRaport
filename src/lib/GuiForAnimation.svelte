<script>
  import { animationsList } from "./AnimationStore.js";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let selected;

  console.log(animationsList);
  let startPlaying = true;

  function toggle() {
    dispatch("updateAnimation", {
      index: selected.id,
      status: startPlaying,
    });
    startPlaying = !startPlaying;
  }
</script>

<div>
  <select bind:value={selected}>
    {#each $animationsList as animation}
      <option value={animation}>
        {animation.name}
      </option>
    {/each}
  </select>

  {#if startPlaying}
    <button on:click={toggle}> Play </button>
  {:else}
    <button on:click={toggle}> Stop </button>
  {/if}
</div>

<style>
  div {
    position: absolute;
    top: 80px;
    left: 80px;
  }
</style>
