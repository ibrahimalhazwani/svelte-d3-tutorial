<script>
    import { each } from "svelte/internal";

    // important export means import!
    export let width;
    export let margin;
    export let yScale;

    let yTicks = yScale.ticks(5);
</script>

<g class="axis y" transform="translate(0, {margin.top})">
    {#each yTicks as tick, index}
        <g class="tick" transform="translate(0, {yScale(tick)})">
            <line
                x1={0}
                x2={width}
                y1={0}
                y2={0}
                stroke={index === 0 ? "#8f8f8f" : "#e5e7eb"}
            />
            <text y={-3}
                >{index === yTicks.length - 1
                    ? `${tick} hours studied`
                    : tick}</text
            >
        </g>
    {/each}
</g>
