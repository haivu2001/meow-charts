<script lang="ts">
    import {fade} from 'svelte/transition';

    export let width = "100%"
    export let height = "100%"
    export let items: any[]
    export let max = 100
    export let yLabels = [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
    export let fill = "darkblue"
    export let gap = 1
    export let columnColors: any[] = []
    export let xLabels: any[] = []
    export let formatter = (x, y) => `${y}`

    let mouseX = -1
    let mouseY = -1
    let mouseItem = 0
    let mouseTime = 0

    $: columnHeights = items.map(x => x / max * 100)
</script>

<svg {width} {height} viewBox="0 0 400 240">
    <svg x="24" y="20" height="220" width="376" viewBox="0 0 {(items.length)*(20*gap)+9} 100"
         preserveAspectRatio="none">
        {#each yLabels.reverse() as label,index}
            <line x1="0" x2="100%" y1={index*100/yLabels.length+0.5} y2={index*100/yLabels.length+0.5} stroke="gray"
                  shape-rendering="crispEdges" stroke-width="0.1px"/>
            <text class="label" x="-2" y="{index*100/yLabels.length+0.5}" alignment-baseline="middle"
                  text-anchor="end">{label}</text>
        {/each}
        {#each items as item, index}
            <g class="column-g" on:mousemove={(x)=>{
                mouseX = x.clientX
                mouseY = x.clientY
                mouseItem = item
                mouseTime = xLabels[index]
            }}
               on:mouseleave={()=> {mouseX = -1;mouseY =-1}}>
                <rect x={21*index+2.5} y={100-columnHeights[index]-0.5}
                      width={18} height={columnHeights[index]} opacity="0.7"
                      fill={columnColors[index]? columnColors[index] : fill} shape-rendering="crispEdges"/>
                <rect x={21*index+2.5} y=0.5
                      width=18 height=99 opacity="0.0"
                      fill={columnColors[index]? columnColors[index] : fill} shape-rendering="crispEdges" class="bulb"/>
                <svg x={21*index+2.5} y="-8" width=18 height="10" viewBox="0 0 40 40" preserveAspectRatio="none">
                    <polygon points="0.5,0.5, 39.5,0.5, 39.5,24.5, 22.5,24.5, 20.5,32.5, 18.5,24.5, 0.5,24.5"
                             stroke="black"
                             stroke-width="0.3px"
                             shape-rendering="geometricPrecision"
                             fill="none" opacity="0" class="bulb"/>
                </svg>
            </g>
        {/each}
        <line x1="0" x2="100%" y1="99.5" y2="99.5" stroke="black" stroke-width="0.1px" shape-rendering="crispEdges"/>
    </svg>
    <svg x="24" y="0" height="20" width="376" viewBox="0 0 {(items.length)*(20*gap)+9} 20"
         preserveAspectRatio="xMinYMid slice">
        {#each xLabels as label,index}
            <text fill="black" alignment-baseline="middle" x={21*index+2.5+9} text-anchor="middle" y=10 class="label">
                {label}
            </text>
        {/each}
    </svg>

</svg>
{#if mouseX > 0}
    <div class="info" transition:fade
         style="position: fixed;left: {mouseX-32}px;top:{mouseY+30}px">{formatter(mouseTime, mouseItem)}</div>
{/if}

<style lang="sass">
  .column-g:hover > .bulb
    opacity: 0.2

  .column-g:hover > svg > .bulb
    opacity: 1.0

  .label
    font: 3px Lucida, sans-serif

  svg > svg
    overflow: visible

  .info
    width: 120px
    height: 60px
    background: white
    border: 2px solid lightgray
    border-radius: 0 5px 15px 5px
    opacity: 50%
    padding: 5px
</style>