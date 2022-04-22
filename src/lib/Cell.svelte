<script>
 export let selected;
 export let onClick;
 export let colors;
 export let elems = new Set();
 $: elemsArr = [...elems];
 $: opacity = (100/Math.ceil(Math.sqrt(elemsArr.length))) + '%';
 $: color = elemsArr.length === 1 ? colors[elemsArr[0]] : colors[0];
</script>

<div
    class="Cell"
    class:selected
    style:background-color="{color}"
    on:click="{onClick}"
>
    {#each [...elems] as elem}
        <div
            class="elem"
            style:background-color="{colors[elem]}"
            style:width="1em"
            style:height="1em"
            style:opacity
        >
            <div class="text">
                {elem}
            </div>
        </div>
    {/each}
</div>

<style>
 .Cell {
     width: 1em;
     height: 1em;
     position: relative;
 }
 .Cell.selected {
     border-color: red;
     z-index: 1;
 }
 .Cell .elem {
     position: absolute;
     display: grid;
     place-items: center;
 }
 .Cell .elem .text {
     font-size: 0.8em;
 }
</style>
