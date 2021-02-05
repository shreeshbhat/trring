<script>
  import AddTimer from './components/AddTimer.svelte';
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: 500px;
    }
  }

</style>

<main>
  <AddTimer></AddTimer>
</main>
