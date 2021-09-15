<script>
  let animations = [];

  // let animations = [
  //   { id: 1, text: `Where did you go to school?` },
  //   { id: 2, text: `What is your mother's name?` },
  //   {
  //     id: 3,
  //     text: `What is another personal fact that an attacker could easily find with Google?`,
  //   },
  // ];

  let selected;

  let isPlaying = true;
  let modelIsLoaded = animations.length > 0;

  function toggle() {
    isPlaying = !isPlaying;
  }
</script>

<div>
  {#if modelIsLoaded}
    <select value={selected}>
      {#each animations as animation}
        <option value={animation}>
          {animation.text}
        </option>
      {/each}
    </select>

    {#if isPlaying}
      <button on:click={toggle}> Pause </button>
    {:else}
      <button on:click={toggle}> Play </button>
    {/if}
  {/if}
</div>

<style>
  div {
    position: absolute;
    top: 80px;
    left: 80px;
  }
</style>
