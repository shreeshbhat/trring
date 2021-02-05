<script>
  import Timer from './Timer.svelte';
  let count = 0;
  function addTimer() {
    count++;
  }
</script>

<button on:click={addTimer}>
  Add Timer
</button>

{#each Array(count) as index}
  <Timer/>
{/each}
<style>
  button {
    margin-bottom: 20px;
  }
</style>
