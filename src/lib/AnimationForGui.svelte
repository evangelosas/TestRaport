<script>
  import { animationsList } from "./AnimationStore.js";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let selected;

  console.log(animationsList);
  let isPlaying = false;

  function toggle() {
    dispatch("updateAnimation", {
      index: selected.id,
      status: isPlaying,
    });
    isPlaying = !isPlaying;
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

  {#if isPlaying}
    <button on:click={toggle}> Stop </button>
  {:else}
    <button on:click={toggle}> Play </button>
  {/if}
</div>

<style>
  div {
    position: absolute;
    top: 80px;
    left: 80px;
  }
</style>
