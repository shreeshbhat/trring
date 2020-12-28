<script>
  export let countdownTime = "";
  let isToggled;
  let distance;
  let id;
  function countDown(value) {
    distance = value;
    id = setInterval(function () {
      distance -= 1000;
      // If the count down is over, write some text
      if (distance <= 0) {
        clearInterval(id);
        id = null;
        isToggled = false;
        countdownTime = "All done!";
      }
    }, 1000);
    return id;
  }

  // Time calculations for days, hours, minutes and seconds
  $: {
    if (distance && distance >= 0) {
      let days = Math.floor(distance / (1000 * 60 * 60 * 24));
      let hours = Math.floor(
        (distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
      );
      let minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      let seconds = Math.floor((distance % (1000 * 60)) / 1000);

      if (days) countdownTime = days + "d " + hours + "h " + minutes + "m " + seconds + "s ";
      else if (hours) countdownTime = hours + "h " + minutes + "m " + seconds + "s ";
      else if (minutes) countdownTime = minutes + "m " + seconds + "s ";
      else countdownTime = seconds + "s ";
    }
  }

  function toggle() {
    isToggled = !isToggled;
    if (isToggled) {
      if (id) {
        id = countDown(distance);
      }
      else {
        id = countDown(5000);
      }
    } else {
      clearInterval(id);
    }
  }
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
      max-width: none;
    }
  }
</style>

<main>
  <button on:click={toggle}>
    {#if !isToggled} Start {:else} Pause {/if}
  </button>
  <p>{countdownTime}</p>
</main>
