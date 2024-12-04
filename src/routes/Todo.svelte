<script>
  import { text } from "@sveltejs/kit";

let todoItem = $state('');
let todoList = $state([]);
let doneList = $state([]);

function addItem(event) {
     event.preventDefault();
     if (todoItem == '') {
          return;
     }
     todoList = [...todoList, {
          text: todoItem,
          done: false
     }];
     todoItem = '';
}
function removeItem(index){
     todoList = todoList.toSpliced(index, 1);
}

function nuke(){
     todoList = [];
}
$effect (() => {
     doneList = todoList.filter((item) => item.done);
})
$inspect(todoList);
$inspect(doneList);

</script>
<form onsubmit={addItem}>
<input type="text" bind:value={todoItem}>
<button type="submit">Add</button>
</form>
<div class="todo-list">
<ul>
     {#each todoList as item, index}
          <li>
               <input type="checkbox" bind:checked={item.done}>
               <span class:done={item.done}>{item.text}</span>
               <button type="button" onclick={() => removeItem(index)}>x</button>
          </li>
     {/each}
</ul>
{#if (todoList.length == 0)}
     <button disabled type="button">Blow the Clouds Away!</button>
{:else}
     <button type="button" onclick={nuke}>Blow the Clouds Away!</button>
{/if}
</div>

{#if (doneList.length > 0)}
<div class="done-list">
     <h3>Done Items</h3>
     {#each doneList as item}
     <li>
          {item.text}
     </li>
     {/each}
</div>
{/if}

<style>
ul {
     list-style: none;
}
</style>