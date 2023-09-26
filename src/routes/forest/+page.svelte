<script>
  import { onMount } from "svelte";
  let src = "images/forest.png";
  let canvas;
  let ctx;
  let forestItems = [
    {
      name: "Grey Wolf",
      x: 316,
      y: 643,
      width: 215,
      height: 100,
      isHighlighted: false,
      found: false,
    },
    {
      name: "White-tailed Deer",
      x: 996,
      y: 596,
      width: 170,
      height: 100,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Blue Jays",
      x: 137,
      y: 140,
      width: 499,
      height: 162,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Spruce Trees",
      x: 200,
      y: 620,
      width: 109,
      height: 111,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Northern Red Oak Trees",
      x: 652,
      y: 147,
      width: 350,
      height: 500,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Grass",
      x: 727,
      y: 686,
      width: 225,
      height: 100,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Redcurrant",
      x: 1270,
      y: 627,
      width: 57,
      height: 67,
      isHighlighted: false,
      found: false,
    },
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
    ctx = canvas.getContext("2d");
    canvas.addEventListener(
      "click",
      function (ctx) {
        var mousePos = updateScore(canvas, ctx);
        // TODO: show item info
      },
      false
    );
    // Add event listeners for hover
    canvas.addEventListener("mousemove", handleMouseMove);
  });

  function handleMouseMove(event) {
    const mousePos = getMousePos(canvas, event);

    // Clear the canvas to remove previous highlights
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Check if the mouse is over any forest item in forestItems2
    for (const item of forestItems) {
      if (
        mousePos.x >= item.x &&
        mousePos.x <= item.x + item.width &&
        mousePos.y >= item.y &&
        mousePos.y <= item.y + item.height
      ) {
        item.isHighlighted = true; // Set the item as highlighted
        drawHighlight(item);
      } else {
        item.isHighlighted = false; // Reset highlighting if not over the item
      }
    }
  }

  function drawHighlight(item) {
    ctx.fillStyle = "rgba(255, 255, 0, 0.1)";
    ctx.fillRect(item.x - 160, item.y - 150, item.width, item.height);
  }

  function getMousePos(canvas, ctx) {
    return {
      x: ctx.clientX,
      y: ctx.clientY,
    };
  }

  function updateScore(canvas, event) {
    const mousePos = getMousePos(canvas, event);

    for (const item of forestItems) {
      if (
        mousePos.x >= item.x &&
        mousePos.x <= item.x + item.width &&
        mousePos.y >= item.y &&
        mousePos.y <= item.y + item.height &&
        !item.found
      ) {
        item.found = true;
        foundItems += 1;
        score = Math.floor((foundItems / forestItems.length) * 100);
      }
    }
  }
</script>

<div class="container">
  <h1>virtual-forest</h1>
  <p>Items found: {foundItems}</p>
  <p>Score: {score}</p>
  <div class="population-counts">
    <p>Population Counts:</p>
    {#each [...populationCounts] as [key, value]}
      <p>{key}: {value}</p>
    {/each}
  </div>
  <canvas bind:this={canvas} width="1200px" height="600px" />
  <audio src="audio/nature.wav" autoplay loop />
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
