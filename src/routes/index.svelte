<script>
 import Cell from '$lib/Cell.svelte';
 const colors = [
     '#08c',
     '#cc0',
     '#0c0',
     '#080',
 ];
 const rules = {
     0: new Set([0, 1]),
     1: new Set([0, 1, 2]),
     2: new Set([1, 2, 3]),
     3: new Set([2, 3]),
 };
 let selected = 0;
 const board = new Array(10).fill().map(row => (
     new Array(10).fill() //.map(_ => Math.floor(Math.random()*colors.length))
 ));
 function setIntersect(a, b) {
     return new Set([...a].filter(item => b.has(item)));
 }
 function neighbors(x, y) {
     function adjacent() {
         return [
             Array.isArray(board[y-1]) && board[y-1][x],
             Array.isArray(board[y])   && board[y][x-1],
             Array.isArray(board[y+1]) && board[y+1][x],
             Array.isArray(board[y])   && board[y][x+1],
         ];
     }
     function diagonal() {
         return [
             Array.isArray(board[y-1]) && board[y-1][x-1],
             Array.isArray(board[y-1]) && board[y-1][x+1],
             Array.isArray(board[y+1]) && board[y+1][x-1],
             Array.isArray(board[y+1]) && board[y+1][x+1],
         ];
     }
     return [...adjacent(), ...diagonal()];
 }
 function step(event) {
     const empty = [];
     for (let x=0; x<10; ++x)
         for (let y=0; y<10; ++y)
             if (typeof board[y][x] !== 'number')
                 empty.push([x, y]);
     if (empty.length === 0)
         return;
     const [x, y] = empty[Math.floor(Math.random() * empty.length)];
     const changes = [];
     const pool = [
         ...neighbors(x, y).filter(cell => typeof cell === 'number')
         .map(elem => rules[elem])
         .reduce(setIntersect, new Set(colors.map((e,i) => i)))
     ];
     const val = pool[Math.floor(Math.random() * pool.length)];
     changes.push({ x, y, val });
     changes.forEach(({ x, y, val }) => {
         board[y][x] = val;
     });
 }
</script>

<div class="container">
    <div class="board">
        {#each board as row, y}
            <div class="row">
                {#each row as cell, x}
                    <Cell
                        color="{colors[cell]}"
                        onClick="{() => board[y][x] = selected}"
                    />
                {/each}
            </div>
        {/each}
    </div>
    <div class="palette">
        {#each colors as color, i}
            <Cell
                {color}
                selected="{selected === i}"
                onClick="{() => selected = i}"
            />
        {/each}
    </div>
    <button on:click={step}>Step</button>
</div>

<style>
 .container {
     display: flex;
     flex-direction: column;
     align-items: center;
     justify-content: center;
     height: 100vh;
     font-size: calc(6vmin);
 }
 .container > * {
     margin: .5em;
 }
 .board {
     display: flex;
     flex-direction: column;
 }
 .palette {
     display: flex;
 }
 .row {
     display: flex;
 }
</style>
