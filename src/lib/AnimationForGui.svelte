<script>
  import { animationsList } from "./AnimationStore.js";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let selected;

  console.log(animationsList);
  let isPlaying = true;
  let modelIsLoaded = isModelLoaded();

  function toggle() {
    dispatch("updateAnimation", {
      index: selected.id,
      status: isPlaying,
    });
    isPlaying = !isPlaying;
  }

  function isModelLoaded() {
    console.log($animationsList);
    return true; //$animationsList.length > 0;
  }
</script>

<div>
  <select disabled={!modelIsLoaded} bind:value={selected}>
    {#each $animationsList as animation}
      <option value={animation}>
        {animation.name}
      </option>
    {/each}
  </select>

  {#if isPlaying}
    <button disabled={!modelIsLoaded} on:click={toggle}> Pause </button>
  {:else}
    <button disabled={!modelIsLoaded} on:click={toggle}> Play </button>
  {/if}
</div>

<style>
  div {
    position: absolute;
    top: 80px;
    left: 80px;
  }
</style>
