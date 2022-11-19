<script>
    import { each } from "svelte/internal";

    // important export means import!
    export let height;
    export let width;
    export let margin;
    export let xScale;

    let xTicks = [0, 25, 50, 75, 100];
</script>

<g class="axis-x" transform="translate(0, {height - margin.bottom})">
    {#each xTicks as tick, index}
        <g class="tick" transform="translate({xScale(tick)}, 0)">
            <line x1={0} x2={0} y1={0} y2={6} stroke="black" />
            <text
                y={6}
                dy={9}
                text-anchor={index === 0 ? "start" : "middle"}
                dominant-baseline="middle">{tick}%</text
            >
        </g>
    {/each}
    <text class="axis-title" y={-9} x={width} text-anchor="end">
        Final Grade &rarr;
    </text>
</g>
