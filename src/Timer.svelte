<script>
  const MAX_SECONDS = 60 * 1;
  let secondsLeft = MAX_SECONDS;
  let roundsLeft = 4;
  let interval;

  function tickDown() {
    secondsLeft -= 1;
    if (secondsLeft == 0) {
      stop();
      roundsLeft -= 1;
      secondsLeft = MAX_SECONDS;
    }
  }
  function start() {
    interval = setInterval(tickDown, 1000);
  }
  function stop() {
    clearInterval(interval);
    interval = null;
  }
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  audio {
    display: none;
  }

  h1 {
    font-size: 4em;
    font-weight: 100;
  }
  h2 {
    font-size: 2em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <!-- <audio preload="auto" controls="none" on:play={playSound} /> -->
  <h1>{Math.floor(secondsLeft / 60)} : {secondsLeft % 60}</h1>
  <h2>Rounds left: {roundsLeft}/4</h2>
  {#if interval}
    <button on:click={stop}>Stop</button>
  {:else}<button on:click={start}>Start</button>{/if}
</main>
