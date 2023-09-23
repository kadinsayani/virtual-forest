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
    var rect = canvas.getBoundingClientRect();
    return {
      x: ctx.clientX - rect.left,
      y: ctx.clientY - rect.top,
    };
  }

  function updateScore() {}
</script>

<canvas bind:this={canvas} width="100%" height="100h" />

<style>
  :global(:root) {
    font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
      Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
      sans-serif;
  }

  canvas {
    background-image: url("images/forest.png");
    height: 100%;
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    width: 100%;
    height: 100vh;
  }
</style>
