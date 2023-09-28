<script>
  import { onMount } from "svelte";
  let src = "images/forest.png";
  let canvas;
  let ctx;
  let forestItems = [
    {
      name: "Grey Wolf",
      x: 110,
      y: 703,
      width: 215,
      height: 100,
      isHighlighted: false,
      found: false,
    },
    {
      name: "White-tailed Deer",
      x: 785,
      y: 639,
      width: 170,
      height: 100,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Blue Jays",
      x: 0,
      y: 192,
      width: 499,
      height: 162,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Spruce Trees",
      x: 2,
      y: 654,
      width: 109,
      height: 111,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Northern Red Oak Trees",
      x: 505,
      y: 197,
      width: 350,
      height: 500,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Grass",
      x: 527,
      y: 722,
      width: 225,
      height: 100,
      isHighlighted: false,
      found: false,
    },
    {
      name: "Redcurrant",
      x: 1064,
      y: 681,
      width: 57,
      height: 67,
      isHighlighted: false,
      found: false,
    },
  ];
  let foundItems = 0;
  let populationCounts = {};
  populationCounts["Grey Wolf"] = 15;
  populationCounts["White-tailed Deer"] = 5;
  populationCounts["Blue Jays"] = 50;
  populationCounts["Spruce Trees"] = 20;
  populationCounts["Northern Red Oak Trees"] = 40;
  populationCounts["Grass"] = 1000;
  populationCounts["Redcurrant"] = 5;

  onMount(() => {
    ctx = canvas.getContext("2d");
    canvas.addEventListener(
      "click",
      function (ctx) {
        var mousePos = updateScore(canvas, ctx);
        clickMousePos(canvas, ctx);
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
    ctx.fillRect(item.x +50, item.y - 190, item.width, item.height);
  }

  function getMousePos(canvas, ctx) {
    return {
      x: ctx.clientX,
      y: ctx.clientY,
    };
  }

  function clickMousePos(canvas, ctx) {
    const x = ctx.clientX;
    const y = ctx.clientY;
    console.log(`${x}, ${y}`);
    return {
      x,
      y,
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
      }
    }
  }

  function sliderOnChange(event) {
    const key = event.target.id;
    const value = populationCounts[key];
    console.log(`${key} slider value: ${value}`);
    if (key === "Grey Wolf") {
      populationCounts["White-tailed Deer"] = Math.floor(value * 0.2);
      return;
    }
    if (key === "White-tailed Deer") {
      populationCounts["Grass"] = Math.floor(value * 0.9);
    }
  }
</script>

<div class="container">
  <h1>Nature Explorer</h1>
  <div class="score-container">
    <p>Items found: {foundItems}/{forestItems.length}</p>
  </div>

  <audio src="audio/nature.wav" autoplay loop />
  <div class="canvas-sliders-container">
    <canvas bind:this={canvas} width="1200px" height="600px" />
    <div class="slider-container">
      <div class="population-label">
        <p><b>Population Counts</b></p>
      </div>
      {#each Object.entries(populationCounts) as [key, value]}
        <div class="slider">
          <input
            type="range"
            id={key}
            name={key}
            min="0"
            max="1000"
            bind:value={populationCounts[key]}
            on:change={sliderOnChange}
          />
          <label for={key}>{key}:{value}</label>
        </div>
      {/each}
    </div>
  </div>
</div>

<style>
  :global(:root) {
    font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
      Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
      sans-serif;
    color: White;
    background-image: linear-gradient(#001e00, #505050);
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
    gap: 15px;
  }

  .slider-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 40px;
  }

  .slider {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .canvas-sliders-container {
    display: flex;
    flex-direction: row;
    gap: 10px;
  }

  .score-container {
    display: flex;
    flex-direction: row;
    gap: 15px;
  }
</style>
