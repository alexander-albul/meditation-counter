<script>
  // Sound source: https://mixkit.co/ 🤗

  import { fade } from "svelte/transition";
  import { onMount } from "svelte";

  import Button from "./components/Button.svelte";

  let goal = 50;
  let count = 0;
  let alertShown = false;
  let done = false;

  const add = () => {
    if (!done && !alertShown) {
      count += 1;
      playAudio(count);
    }

    if (count === goal) {
      done = true;
    }
  };

  // For the sound to play properly, you have to declare a variable every time
  // and stop the sound in a not very elegant way

  const playAudio = count => {
    if (count === goal) {
      let success = new Audio("./src/success.wav");
      return success.play();
    }

    if (count % 10 === 0 && count !== 0) {
      let longTap = new Audio("./src/long-tap.wav");
      return longTap.play();
    }

    if (count !== 0) {
      let tap = new Audio("./src/tap.mp3");
      tap.currentTime = 0;
      tap.pause();
      tap.play();
    }
  };

  const showAlert = () => {
    let alertSound = new Audio("./src/alert.mp3");
    alertShown = true;
    alertSound.play();
  };

  const hideAlert = () => {
    alertShown = false;
  };

  const reset = () => {
    count = 0;
    done = false;
    alertShown = false;
  };
</script>



<svelte:head>
  <title>{count > 0 ? count + ' - ' : ''}Meditation Counter</title>
</svelte:head>

<main on:click|self={add} class:faded={alertShown}>
  <span class="count">{count}</span>
  {#if done}
    <p class="count">Done!</p>
  {/if}
  <div class="buttons" >
    {#if count && !alertShown}
      <Button on:click={showAlert} secondary title="Reset"/>
    {/if}
    {#if alertShown}
      <Button on:click={reset} secondary title="Reset for sure"/>
      <Button on:click={hideAlert} title="Cancel"/>
    {/if}
  </div>
  <label>Goal:
    <input bind:value={goal} type="number" min="0" max="999">
  </label>
</main>

<Button/>


<style>
  :global(*) {
    box-sizing: border-box;
  }

  :global(:root) {
    --semi-transparent: rgba(255 255 255 / 0.5);
  }

  :global(body) {
    margin: 0;
  }

  main {
    position: relative;
    display: grid;
    align-content: center;
    place-items: center;
    padding: 1rem;
    min-height: 100vh;
    font-family: sans-serif;
    color: white;
    background: #047857;
    user-select: none;
    transition: background 0.2s;
  }

  .faded {
    background: DarkSlateGray;
  }

  .count {
    font-size: 4rem;
    font-weight: bold;
    text-align: center;
    text-shadow: 2px 2px 2px black;
    transition-property: transform, opacity, filter, text-shadow;
    transition-duration: 0.1s;
    transition-timing-function: linear;
    user-select: none;
    pointer-events: none;
  }

  main:active .count {
    text-shadow: 1px 1px 1px black;
    opacity: 0.8;
    transform: scale(0.85);
  }

  .buttons {
    position: absolute;
    top: 1rem;
    right: 1rem;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    gap: 0.5rem;
    user-select: none;
  }

  label {
    position: absolute;
    top: 1rem;
    left: 1rem;
    padding-inline-start: 0.5rem;
    border: 1px solid var(--semi-transparent);
    background: transparent;
    color: white;
    border-radius: 0.25rem;
  }

  input {
    --padding-x: 0.5rem;
    padding-inline: var(--padding-x);
    padding-block: 0.5rem;
    width: calc(var(--padding-x) * 2 + 4ch);
    font-size: 1rem;
    border: 1px solid transparent;
    background: transparent;
    color: white;
  }

  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  /* Firefox */
  input[type="number"] {
    -moz-appearance: textfield;
  }

  p {
    margin: 0;
  }
</style>