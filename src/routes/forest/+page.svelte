<script>
  import { onMount } from "svelte";
  import GreyWolfModal from './GreyWolfModal.svelte';
  import BlueJayModal from './BlueJayModal.svelte';
  import DeerModal from './DeerModal.svelte';
  import GrassModal from './GrassModal.svelte';
  import OakModal from './OakModal.svelte';
  import RedCurrantModal from './RedCurrantModal.svelte';
  import SpruceModal from './SpruceModal.svelte';

  let src = "images/forest.png";
  let canvas;
  let ctx;

  let xMax = 0;
  let xMin = 0;
  let yMax = 0;
  let yMin = 0;

  let activateGreyWolf = false;
  let activateDeer = false;
  let activateBlueJay = false;
  let activateSpruce = false;
  let activateOak = false;
  let activateGrass = false;
  let activateRedCurrant = false;


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

    //Add event listener for modal boxes
    canvas.addEventListener("click", activateModal);

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


  function activateModal(event){
    const mousePos = getMousePos(canvas, event);

  // Check if the mouse is over any forest item in forestItems2
    for (const item of forestItems) {
      if (
        mousePos.x >= item.x &&
        mousePos.x <= item.x + item.width &&
        mousePos.y >= item.y &&
        mousePos.y <= item.y + item.height
      ) 
      {
        xMax = item.x + item.width;
        xMin = item.x;
        yMax = item.y + item.height;
        yMin = item.y;
        //activateGreyWolf = true;
        if(item.name === "Grey Wolf"){
          activateGreyWolf = true;
          drawHighlight(item);
        }
        else if(item.name === "White-tailed Deer"){
          activateDeer = true;
        }
        else if(item.name === "Blue Jays"){
          activateBlueJay = true;
        }
        else if(item.name === "Spruce Trees"){
          activateSpruce = true;
        }
        else if(item.name === "Northern Red Oak Trees"){
          activateOak = true;
        }
        else if(item.name === "Grass"){
          activateGrass = true;
        }
        else if(item.name === "Redcurrant"){
          activateRedCurrant = true;
        }
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


<!--Grey wolf modal content-->
{#if activateGreyWolf}
<button class="greyWolf-button">  </button>

  <GreyWolfModal bind:activateGreyWolf>
    <h2>Gray Wolf </h2>
    <p>Gray wolf, (Canis lupus), also called timber wolf, largest wild member of the dog family (Canidae). It inhabits vast areas of the Northern Hemisphere. Between 5 and 24 subspecies of gray wolves are recognized in North America and 7 to 12 are recognized in Eurasia, with 1 in Africa. Wolves were domesticated several thousand years ago, and selective breeding produced dogs.
    </p>

    <p>For more information, click the site below</p>

    <a href="https://www.britannica.com/animal/gray-wolf">https://www.britannica.com/animal/gray-wolf</a>
  </GreyWolfModal>
  {/if}


  <!--Blue Jay modal content-->
{#if activateBlueJay}
<button class="BlueJay-button">  </button>


<BlueJayModal bind:activateBlueJay>
  <h2>Blue Jay</h2>
  <p>This is the BlueJay modal content.</p>

  <p>This common, large songbird is familiar to many people, with its perky crest; blue, white, and black plumage; and noisy calls. Blue Jays are known for their intelligence and complex social systems with tight family bonds. Their fondness for acorns is credited with helping spread oak trees after the last glacial period.
  </p>

  <p>For more information, click the site below</p>

  <a href="https://www.allaboutbirds.org/guide/Blue_Jay/overview#">https://www.allaboutbirds.org/guide/Blue_Jay/overview#</a>
</BlueJayModal>
{/if}




<!--Deer modal content-->
{#if activateDeer}
<button class="Deer-button">  </button>


<DeerModal bind:activateDeer>
  <h2>White Tailed Deer</h2>
  <p>The white-tailed deer (Odocoileus virginianus), also known as the Virginia deer, is a common American species within the Cervidae family. Their range extends from the Arctic Circle in western Canada to Peru and Bolivia, spanning 18 degrees south of the Equator. They are recognized by the long white hair on their tail and rump, resembling a signaling flag during flight. These deer are considered part of the New World deer subfamily. 
    .</p>


  <a href="https://www.britannica.com/animal/white-tailed-deer">https://www.britannica.com/animal/white-tailed-deer</a>
</DeerModal>
{/if}


<!--Grass modal content-->
{#if activateGrass}
<button class="Grass-button">  </button>


<GrassModal bind:activateGrass>
  <h2>Grass</h2>
  <p>grass, any of many low, green, nonwoody plants belonging to the grass family (Poaceae), the sedge family (Cyperaceae), and the rush family (Juncaceae). There are many grasslike members of other flowering plant families, but only the approximately 10,000 species in the family Poaceae are true grasses.</p>


  <a href="https://www.britannica.com/plant/grass">https://www.britannica.com/plant/grass</a>
</GrassModal>
{/if}




<!--Oak modal content-->
{#if activateOak}
<button class="Oak-button">  </button>


<OakModal bind:activateOak>
  <h2>Northern Read Oak</h2>
  <p>The Northern Red Oak is a quite tolerant species to urban weather conditions, powerful winds. It can be expected to grow in hardiness zones of 3-8. The canopy of the trees looks excellent when the heavy winds are blowing. The tree does well in these hardy zones with an excellent soil variety, and its main use is for landscaping purposes. 
  </p>


  <a href="https://localtreeestimates.com/northern-red-oak/">https://localtreeestimates.com/northern-red-oak/</a>
</OakModal>
{/if}


<!--RedCurrant modal content-->
{#if activateRedCurrant}
<button class="RedCurrant-button">  </button>


<RedCurrantModal bind:activateRedCurrant>
  <h2>Red Currant</h2>
  <p>Ribes triste (Red Currant) is a species of shrub in the family gooseberries. They have a self-supporting growth form. They are native to Canada, The Contiguous United States, North America, United States, and Alaska. They have simple, broad leaves and purple flowers. Individuals can grow to 5 feet..</p>


  <a href="https://eol.org/pages/583216">https://eol.org/pages/583216</a>
</RedCurrantModal>
{/if}




<!--Spruce modal content-->
{#if activateSpruce}
<button class="Spruce-button">  </button>


<SpruceModal bind:activateSpruce>
  <h2>Spruce</h2>
  <p>The ornamental and timber trees known as spruces are native to the temperate and cold regions of the Northern Hemisphere. These cone-bearing evergreen trees are usually pyramid-shaped with whorled branches and thin, scaly bark. When the trees grow singly, their lower branches often touch the ground. The cones are egg-shaped or cylindrical. The spruces vary in height from about 50 to 150 feet (15 to 46 meters). 
  </p>


  <a href="https://kids.britannica.com/students/article/spruce/277192">https://kids.britannica.com/students/article/spruce/277192</a>
</SpruceModal>
{/if}




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
