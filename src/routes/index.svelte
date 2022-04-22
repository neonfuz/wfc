<script>
 import Cell from '$lib/Cell.svelte';
 const colors = [
     '#ddd',
     '#08c',
     '#cc0',
     '#0c0',
     '#080',
 ];
 const rules = {
     0: new Set([0]),
     1: new Set([1, 2]),
     2: new Set([1, 2, 3]),
     3: new Set([2, 3, 4]),
     4: new Set([3, 4]),
 };
 let selected = 0;
 const board = new Array(10).fill().map(row => (
     new Array(10).fill(new Set(colors.slice(1).map((c,i) => i+1)))
 ));
 function setIntersect(a, b) {
     return new Set([...a].filter(item => b.has(item)));
 }
 function neighbors(x, y) {
     return [
         // adjacent
         Array.isArray(board[y-1]) && board[y-1][x],
         Array.isArray(board[y])   && board[y][x-1],
         Array.isArray(board[y+1]) && board[y+1][x],
         Array.isArray(board[y])   && board[y][x+1],
         //diagonal
         Array.isArray(board[y-1]) && board[y-1][x-1],
         Array.isArray(board[y-1]) && board[y-1][x+1],
         Array.isArray(board[y+1]) && board[y+1][x-1],
         Array.isArray(board[y+1]) && board[y+1][x+1],
     ];
 }
 function neighborGen(x, y) {
     if(Array.isArray(board[y-1])) board[y-1][x];
     if(Array.isArray(board[y])  ) board[y][x-1];
     if(Array.isArray(board[y+1])) board[y+1][x];
     if(Array.isArray(board[y])  ) board[y][x+1];
     if(Array.isArray(board[y-1])) board[y-1][x-1];
     if(Array.isArray(board[y-1])) board[y-1][x+1];
     if(Array.isArray(board[y+1])) board[y+1][x-1];
     if(Array.isArray(board[y+1])) board[y+1][x+1];
 }
 function setBoard(x, y, elems) {
     board[y][x]
 }
 function step(event) {
     const empty = [];
     for (let x=0; x<10; ++x)
         for (let y=0; y<10; ++y)
             if (board[y][x] === 0)
                 empty.push([x, y]);
     if (empty.length === 0)
         return;
     const [x, y] = empty[Math.floor(Math.random() * empty.length)];
     const changes = [];
     const pool = [
         ...neighbors(x, y)
         .filter(cell => typeof cell === 'number')
         .filter(cell => cell !== 0)
         .map(elem => rules[elem])
         .reduce(setIntersect, new Set(colors.map((e,i) => i)))
     ];
     const val = pool[Math.floor(Math.random() * pool.length)];
     changes.push({ x, y, val });
     changes.forEach(({ x, y, val }) => {
         board[y][x] = val;
     });
 }
 let interval = null;
 function stop() {
     clearInterval(interval);
 }
 function start() {
     stop();
     interval = setInterval(() => {
         step();
     }, 200);
 }
</script>

<div class="container">
    <div class="board">
        {#each board as row, y}
            <div class="row">
                {#each row as cell, x}
                    <Cell
                        {colors}
                        elems="{cell}"
                        onClick="{() => board[y][x] = new Set([selected])}"
                    />
                {/each}
            </div>
        {/each}
    </div>
    <div class="palette">
        {#each colors as color, i}
            <Cell
                {colors}
                elems={new Set([i])}
                selected="{selected === i}"
                onClick="{() => selected = i}"
            />
        {/each}
    </div>
    <div class="buttonBar">
        <button on:click={start}>Start</button>
        <button on:click={stop}>Stop</button>
        <button on:click={step}>Step</button>
    </div>
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
