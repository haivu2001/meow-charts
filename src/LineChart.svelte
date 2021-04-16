<script lang="ts">
    import {fade} from 'svelte/transition'

    export let width = "100%"
    export let height = "100%"
    export let items: any[]
    export let fill = "darkred"
    export let xLabels: any[]
    export let yLabels = [0, 10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
    export let max = 100
    export let formatter = (x, y) => `${y}`

    let dotHeights = items.map(x => x / max * 100)
    let hoverValue = -1
    let hoverHeight = -1
    $: hoverHeight = hoverValue / max * 100
    let mouseX = -1
    let mouseY = -1
    let mouseItem = null
    let mouseTime = null
</script>

<svg {width} {height} viewBox="0 0 400 240">
    <svg x="24" y="16" width="376" height="204"
         viewBox="0 0 {items.length*10} 100"
         preserveAspectRatio="none" style="overflow: visible">
        {#each items as item,index}
            <g on:mousemove={(x)=>{
                mouseX = x.clientX
                mouseY = x.clientY
                mouseItem = item
                mouseTime = xLabels[index]
                hoverValue = item
            }} on:mouseleave={_ => mouseX=-1}>
                <ellipse rx="0.5" ry="{0.5*items.length/10*376/220}" {fill} cx={(0.5+index)*10+0.5}
                         cy={100-0.5-dotHeights[index]}/>
                {#if dotHeights[index + 1]}
                    <line stroke={fill} x1={(0.5+index)*10+0.5} x2={(1.5+index)*10+0.5} y1={100-0.5-dotHeights[index]}
                          y2={100-0.5-dotHeights[index+1]}
                          stroke-width="0.25px"/>
                {/if}
                <rect x={(0.5+index)*10+0.3} y="0" height="100" width="0.2" class="bulb" opacity="0"/>
                <rect x={(0.5+index)*10-2.5} y="0" height="100" width="6" opacity="0"/>
            </g>
        {/each}
        {#each yLabels as label,index}
            <rect width={10*items.length} height="0.05px" x="0" y={100-(index+1)*(100/yLabels.length)+0.5}/>
            <text class="label" alignment-baseline="middle" x="-0.5" y={100-(index+1)*(100/yLabels.length)+0.5}
                  text-anchor="end">{label}</text>
        {/each}
        <rect width={10*items.length} height="0.2px" x="0" y="99.5"/>
        <rect width={10*items.length} height="0.1px" x="0" y={100-hoverHeight-0.5}/>
        {#each xLabels as label,index}
            <text y="104" x={(index+0.5)*10} text-anchor="middle" class="label">{label}</text>
        {/each}
    </svg>
</svg>
{#if mouseX > 0}
    <div class="info" transition:fade
         style="position: fixed;left: {mouseX-32}px;top:{mouseY+30}px">{formatter(mouseTime, mouseItem)}</div>
{/if}

<style lang="sass">
  g:hover > .bulb
    opacity: 50%
    fill: gray

  .label
    font: 2.2px Lucida, sans-serif

  .info
    width: 120px
    height: 60px
    background: white
    border: 2px solid lightgray
    border-radius: 0 5px 15px 5px
    opacity: 50%
    padding: 5px

</style>