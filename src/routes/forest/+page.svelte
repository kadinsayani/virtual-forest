<script>
  import { onMount } from "svelte";
  let src = "images/forest.png";
  let canvas;
  let forestItems = [
    "Grey Wolf",
    "White-tailed Deer",
    "Blue Jays",
    "Spruce Trees",
    "Northern Red Oak Trees",
    "Grass",
    "Redcurrant",
  ];
  let foundItems = 0;
  let score = (foundItems / forestItems.length) * 100;
  let populationCounts = new Map();
  populationCounts.set("Grey Wolf", 10);
  populationCounts.set("White-tailed Deer", 25);
  populationCounts.set("Blue Jays", 100);
  populationCounts.set("Spruce Trees", 50);
  populationCounts.set("Northern Red Oak Trees", 50);
  populationCounts.set("Grass", 1000);
  populationCounts.set("Redcurrant", 10);

  onMount(() => {
    const ctx = canvas.getContext("2d");
    canvas.addEventListener(
      "click",
      function (ctx) {
        var mousePos = getMousePos(canvas, ctx);
        alert(mousePos.x + "," + mousePos.y);
      },
      false
    );
  });

  function getMousePos(canvas, ctx) {
    return {
      x: ctx.clientX,
      y: ctx.clientY,
    };
  }

  function updateScore() {}
</script>

<div class="container">
  <h1>virtual-forest</h1>
  <div class="population-counts">
    <p>Population Counts:</p>
    {#each [...populationCounts] as [key, value]}
      <p>{key}: {value}</p>
    {/each}
  </div>
  <canvas bind:this={canvas} width="600px" height="1200px" />
  <div class="slider-container">
    {#each [...populationCounts] as [key, value]}
      <div class="slider">
        <input type="range" id={key} name={key} min="0" max="1000" />
        <label for={key}>{key}</label>
      </div>
    {/each}
  </div>
</div>

<style>
  :global(:root) {
    font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
      Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
      sans-serif;
  }

  canvas {
    background-image: url("images/forest.png");
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    width: 1200px;
    height: 600px;
  }

  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .population-counts {
    display: flex;
    flex-direction: row;
  }

  .slider-container {
    display: flex;
    flex-direction: row;
    justify-content: center;
    gap: 50px;
  }

  .slider {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>
