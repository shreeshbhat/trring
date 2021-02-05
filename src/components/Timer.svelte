
<script>
  let isToggled;
  let distance;
  let id;
  let percentCompleted = 0;
  let initialTime = 5000;

  export let countdownTime = calculateCountdownTime(initialTime);

  function countDown(value) {
    initialTime = value;
    id = setInterval(function () {
      distance -= 1000;
      percentCompleted = ((initialTime - distance) / initialTime) * 100;
      // If the count down is over, write some text
      if (distance <= 0) {
        clearInterval(id);
        id = null;
        isToggled = false;
        percentCompleted = 100;
        countdownTime = "All done!";
        loadAudio();
      }
    }, 1000);
    return id;
  }

  function calculateCountdownTime(value) {
    let countDown;
    let days = Math.floor(value / (1000 * 60 * 60 * 24));
    let hours = Math.floor(
      (value % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
    );
    let minutes = Math.floor((value % (1000 * 60 * 60)) / (1000 * 60));
    let seconds = Math.floor((value % (1000 * 60)) / 1000);

    if (days)
    countDown =
        days + "d " + hours + "h " + minutes + "m " + seconds + "s";
    else if (hours)
    countDown = hours + "h " + minutes + "m " + seconds + "s";
    else if (minutes) countDown = minutes + "m " + seconds + "s";
    else countDown = seconds + "s";
    return countDown
  }

  // Time calculations for days, hours, minutes and seconds
  $: {
    if (distance && distance >= 0) {
      countdownTime = calculateCountdownTime(distance);
    }
  }

  function toggle() {
    isToggled = !isToggled;
    if (id == null) {
      percentCompleted = 0;
    }
    if (isToggled) {
      distance = id? distance : initialTime;
      id = countDown(initialTime);
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

<div class="container">
  <p>{countdownTime}</p>
  <progress id="time" value={percentCompleted} max="100" />
  <button on:click={toggle}>
    {#if !isToggled}Start{:else}Pause{/if}
  </button>
</div>

<style>
  .container {
    border: 1px solid snow;
    border-radius: 4px;
    padding: 10px;
    display: flex;
    flex-direction: column;
    margin-bottom: 30px;
  }

  button {
    margin: 0;
  }
  p {
    color: snow;
  }

  progress {
    width: 100%;
    margin-bottom: 10px;
  }
</style>
