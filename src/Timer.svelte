<script>
  const MAX_SECONDS = 3; //60 * 7;
  const MAX_ROUNDS = 2; //4
  let secondsLeft = MAX_SECONDS;
  let roundsLeft = MAX_ROUNDS;
  let interval;

  const cheerSound = new Audio("../sounds/cheer2.mp3");
  const victoryFiles = [
    "../sounds/footballcrowd.mp3",
    "../sounds/footballcrowd2.mp3",
    "../sounds/tottnu8.mp3",
    "../sounds/yeehaw.mp3",
  ];
  const randomIndex = Math.floor(Math.random() * 10) % victoryFiles.length;
  const victorySound = new Audio(victoryFiles[randomIndex]);
  console.log(randomIndex, victoryFiles[randomIndex]);

  function tickDown() {
    secondsLeft -= 1;
    if (secondsLeft == 0) {
      stop();
      roundsLeft -= 1;
      if (roundsLeft == 0) {
        victorySound.play();
      } else {
        cheerSound.play();
        secondsLeft = MAX_SECONDS;
      }
    }
  }

  function start() {
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
  h2 {
    font-size: 2em;
    font-weight: 100;
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
  <!-- <audio preload="auto" controls="none" bind:this={audioRef}>Your browser does
    not support audio</audio> -->

  <h1>
    {toStringTwoChars(Math.floor(secondsLeft / 60))} : {toStringTwoChars(secondsLeft % 60)}
  </h1>
  <h2>Rounds left: {roundsLeft}/{MAX_ROUNDS}</h2>
  {#if roundsLeft === 1}
    <h2 class="lastRound">Last round!!</h2>
  {:else if roundsLeft === 0}
    <h2>Done 🛀</h2>
  {/if}
  {#if interval}
    <button on:click={stop}>Stop</button>
  {:else}<button on:click={start}>Start</button>{/if}
</main>
