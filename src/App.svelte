<script>
  // import data
  import data from "./data/data.js";

  // import libraries
  import { scaleLinear } from "d3-scale";
  import { max, sort } from "d3-array";

  // import components
  import AxisX from "./lib/AxisX.svelte";
  import AxisY from "./lib/AxisY.svelte";
  import Tooltip from "./lib/Tooltip.svelte";

  let width = 400;
  let height = 400;
  let radius = 10;
  let hoveredData;

  const margin = { top: 20, right: 20, left: 0, bottom: 20 };

  $: xScale = scaleLinear()
    .domain([0, 100])
    .range([0, width - margin.left - margin.right]);

  console.log(xScale);

  $: yScale = scaleLinear()
    .domain([0, max(data, (d) => d.hours)])
    .range([height - margin.top - margin.bottom, 0]); // if you scale the value in a scatterplot with d3 you have to flip this value
</script>

<h1>Study longer, score better</h1>

<div class="chart-container" bind:clientWidth={width}>
  <svg
    {width}
    {height}
    on:mouseleave={() => {
      hoveredData = null;
    }}
  >
    <AxisX {height} {width} {xScale} {margin} />
    <AxisY {width} {yScale} {margin} />

    <g class="inner-chart" transform="translate({margin.left} {margin.top})">
      <!-- #each, svelte command to loop over the data -->
      {#each data.sort((a, b) => a.grade - b.grade) as d, index}
        <circle
          cx={xScale(d.grade)}
          cy={yScale(d.hours)}
          r={hoveredData && hoveredData == d ? radius * 2 : radius}
          fill="orange"
          stroke="black"
          opacity={hoveredData ? (hoveredData == d ? "1" : ".45") : ".85"}
          on:mouseover={() => {
            hoveredData = d;
          }}
          on:focus={() => {
            hoveredData == d;
          }}
        />
      {/each}
    </g>
  </svg>

  {#if hoveredData}
    <Tooltip data={hoveredData} {xScale} {yScale} {width} />
  {/if}
</div>

<style>
  .chart-container {
    position: relative;
  }

  circle {
    transition: all 300ms ease;
    cursor: pointer;
  }

  circle:focus {
    outline: none;
  }

  h1 {
    font-size: 1.5rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
  }
</style>
