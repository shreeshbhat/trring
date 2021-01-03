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
        loadAudio();
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

      if (days)
        countdownTime =
          days + "d " + hours + "h " + minutes + "m " + seconds + "s";
      else if (hours)
        countdownTime = hours + "h " + minutes + "m " + seconds + "s";
      else if (minutes) countdownTime = minutes + "m " + seconds + "s";
      else countdownTime = seconds + "s";
    }
  }

  function toggle() {
    isToggled = !isToggled;
    if (isToggled) {
      if (id) {
        id = countDown(distance);
      } else {
        id = countDown(5000);
      }
    } else {
      clearInterval(id);
    }
  }

  function initAudio() {
    try {
      window.audioCtx = window.AudioContext || window.webkitAudioContext;
      let audioCtx = new AudioContext();
      return audioCtx;
    } catch (e) {
      alert("Web Audio API is not supported in this browser");
    }
  }

  function loadAudio() {
    let url = "/assets/Teemo-laugh.mp3";
    let request = new XMLHttpRequest();
    request.open("GET", url, true);
    request.responseType = "arraybuffer";
    request.onload = function () {
      let context = initAudio();
      context.decodeAudioData(
        request.response,
        function (buffer) {
          var source = context.createBufferSource();
          source.buffer = buffer;
          source.connect(context.destination);
          source.start(0);
        },
        function () {
          alert("Failed to decode audio file");
        }
      );
    };
    request.send();
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
    {#if !isToggled}Start{:else}Pause{/if}
  </button>
  <p>{countdownTime}</p>
</main>
