<script>
  let selectedMinutes = 7;
  let selectedSeconds = 0;
  let selectedRounds = 4;
  let roundsLeft = selectedRounds;
  let hasFinalizedTimings = false;

  let interval;
  let videoRef;

  function calculateTotalSeconds() {
    return selectedMinutes * 60 + selectedSeconds;
  }

  let secondsLeft = calculateTotalSeconds();

  const cheerSound = new Audio("./sounds/cheer2.mp3");
  const victoryFiles = [
    "./sounds/footballcrowd.mp3",
    "./sounds/footballcrowd2.mp3",
    "./sounds/tottnu8.mp3",
    "./sounds/yeehaw.mp3",
  ];
  const randomIndex = Math.floor(Math.random() * 10) % victoryFiles.length;
  const victorySound = new Audio(victoryFiles[randomIndex]);

  function tickDown() {
    secondsLeft -= 1;
    if (secondsLeft == 0) {
      stop();
      roundsLeft -= 1;
      if (roundsLeft == 0) {
        victorySound.play();
      } else {
        cheerSound.play();
        secondsLeft = calculateTotalSeconds();
      }
    }
  }

  function start() {
    if (!hasFinalizedTimings) {
      hasFinalizedTimings = true;
      secondsLeft = calculateTotalSeconds();
      roundsLeft = selectedRounds;
    }
    interval = setInterval(tickDown, 1000);
  }

  function stop() {
    clearInterval(interval);
    interval = null;
  }

  function toStringTwoChars(num) {
    if (num < 10) {
      return `0${num}`;
    } else {
      return `${num}`;
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

  h1 {
    font-size: 4em;
    font-weight: 100;
  }
  h1 input {
    font-weight: 100;
    width: 100px;
  }
  h2 {
    font-size: 2em;
    font-weight: 100;
  }
  h2 input {
    font-weight: 100;
    width: 70px;
  }

  input {
    padding: 0;
    border: none;
    text-align: center;
  }

  button {
    font-size: 2em;
    font-weight: 100;
    background-color: white;
  }

  button:hover {
    background-color: rgb(128, 200, 255);
    color: white;
  }

  .lastRound {
    color: red;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  {#if hasFinalizedTimings}
    <h1>
      {toStringTwoChars(Math.floor(secondsLeft / 60))} : {toStringTwoChars(secondsLeft % 60)}
    </h1>
  {:else}
    <h1>
      <input type="number" size="2" min="0" bind:value={selectedMinutes} />:<input
        type="number"
        size="2"
        max="59"
        min="0"
        bind:value={selectedSeconds} />
    </h1>
  {/if}
  {#if interval}
    <!-- svelte-ignore a11y-media-has-caption -->
    <video src="./videos/workout.mp4" width="300" autoplay="true" loop="true" />
  {/if}
  {#if hasFinalizedTimings}
    <h2>Rounds left: {roundsLeft}/{selectedRounds}</h2>
  {:else}
    <h2>
      Rounds: <input type="number" size="2" min="1" bind:value={selectedRounds} />
    </h2>
  {/if}
  {#if roundsLeft === 1}
    <h2 class="lastRound">Last round!!</h2>
  {:else if roundsLeft === 0}
    <h2>Done 🛀</h2>
  {/if}
  {#if interval}
    <button on:click={stop}>Stop</button>
  {:else}<button on:click={start}>Start</button>{/if}
</main>
