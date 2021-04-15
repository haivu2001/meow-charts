<script lang="ts">
    export let width = 400
    export let height = 240
    export let items: any[]
    export let max = 100
    export let yLabels = [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
    export let fill = "darkblue"
    export let gap = 10
    export let xOffsets = items.map(() => 0)
    export let columnColors: any[] = []
    export let xLabels: any[] = []
    export let formatter = x => `${x}%`
    let x1 = 30 + 0.5
    let x2 = width - 10 + 0.5
    const yToBottom = 24
    let columnHeights = items.map(x => x / max * (height - yToBottom) * (yLabels.length / (yLabels.length + 1)) - 1)
</script>

<svg {width} {height}>
    <!--Grid lines-->
    <line {x1} {x2} y1="{height-yToBottom+0.5}" y2="{height-yToBottom+0.5}" stroke="black"
          shape-rendering="crispEdges" stroke-width="0.5"/>
    <text class="small" x="5" y="{height-yToBottom+0.5-height/yLabels.length*-0.15}">{yLabels[0]}</text>
    {#each yLabels.slice(1) as item,index}
        <line {x1} {x2} shape-rendering="crispEdges" stroke-width="0.5"
              y1="{height-yToBottom-((height-yToBottom)/yLabels.length)*(index+1)+0.5}"
              y2="{height-yToBottom-((height-yToBottom)/yLabels.length)*(index+1)+0.5}"
              stroke="black" opacity="0.3"/>
        <text class="small" x="5" y="{height-yToBottom-((height-yToBottom)/yLabels.length)*(index+1)}"
              alignment-baseline="central">{item}</text>
    {/each}
    <!--Columns-->
    {#each items as item,index}
        <g class="column-container">
            <rect x="{x1+gap+(width-40)/items.length*index+xOffsets[index]}"
                  y="{height-yToBottom-columnHeights[index]+0.5}"
                  width="{(width-40)/items.length-gap*2 }" height="{columnHeights[index]}"
                  fill="{columnColors[index]?columnColors[index]:fill}" opacity="0.7"
                  class="column"/>
            <rect x="{x1+gap+(width-40)/items.length*index+xOffsets[index]}"
                  y="0"
                  width="{(width-40)/items.length-gap*2 }" height="{height-yToBottom}"
                  fill="{columnColors[index]?columnColors[index]:fill}" opacity="0.0"
                  class="litter"/>
            <foreignObject height="35" width="{(width-40)/items.length-gap*2 }" opacity="0" class="dataDisplay"
                           x="{x1+gap+(width-40)/items.length*index+xOffsets[index]}"
                           y="{height-yToBottom-columnHeights[index]-18}">
                <div class="small" style="padding: 3px">{formatter(item)}</div>
            </foreignObject>
            <foreignObject height="35" width="{(width-40)/items.length-gap*2}"
                           x="{x1+gap+(width-40)/items.length*index+xOffsets[index]}">
                {#if xLabels[index]}
                    <div class="small x-label">{xLabels[index]}</div>
                {/if}
            </foreignObject>
        </g>
    {/each}
</svg>

<style lang="sass">
  .small
    font: 10px Lucida, sans-serif
    text-align: center

  .x-label
    border: 1px solid lightgray
    padding: 1px
    border-radius: 10%

  .column-container:hover > foreignObject > .x-label
    border: 1px solid gray

  .column-container:hover > .column
    opacity: 0.9

  .column-container:hover > .litter
    opacity: 0.05

  .column-container:hover > .dataDisplay
    opacity: 1.0
</style>