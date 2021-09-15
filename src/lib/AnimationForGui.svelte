<script>
  let animations = [
    { id: 1, text: `Where did you go to school?` },
    { id: 2, text: `What is your mother's name?` },
    {
      id: 3,
      text: `What is another personal fact that an attacker could easily find with Google?`,
    },
  ];

  let selected;

  let isPlaying = true;

  function handleSubmit() {
    alert(`answered question ${selected.id} (${selected.text}) with `);
  }
</script>

<div>
  <select value={selected}>
    {#each animations as animation}
      <option value={animation}>
        {animation.text}
      </option>
    {/each}
  </select>

  <button disabled={!isPlaying} type="submit"> Play </button>
  <!-- Put an if statement for name -->
</div>
<style>
div {
  position: absolute;
  top: 80px;
  left: 80px;
}

</style>
