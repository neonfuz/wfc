<script>
 export let selected;
 export let color;
 export let onClick;
</script>

<div class="Cell" {...$$props}
     class:selected
     style:background-color="{color}"
     on:click="{onClick}"
>
    <slot></slot>
</div>

<style>
 .Cell {
     width: 1em;
     height: 1em;
     outline: solid white 3px;
     background-color: #888;
 }
 .Cell.selected {
     outline: solid red 3px;
     z-index: 1;
 }
</style>
