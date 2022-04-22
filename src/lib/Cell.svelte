<script>
 export let selected;
 export let onClick;
 export let colors;
 export let elems = new Set();
 $: elemsArr = [...elems];
 $: elemSize = (1/Math.ceil(Math.sqrt(elemsArr.length)))-.1;
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
            style:width="{elemSize}em"
            style:height="{elemSize}em"
        />
    {/each}
</div>

<style>
 .Cell {
     width: 1em;
     height: 1em;
     background-color: pink;
     display: flex;
     flex-wrap: wrap;
     align-items: center;
     justify-content: center;
 }
 .Cell.selected {
     border-color: red;
     z-index: 1;
 }
 .Cell .elem {
     border-radius: 50%;
 }
</style>
