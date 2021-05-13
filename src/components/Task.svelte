<script>
  import { createEventDispatcher } from 'svelte';
  import axios from 'axios';

  import global from '../global.js';

  export let task;

  const dispatch = createEventDispatcher();

  let showDel = false;

  function toggleDel() {
    showDel = !showDel;
  }

  async function deleteItem(id) {
    const resDel = await axios.delete(global.baseUrl + '/tasks/' + id);

    dispatch('taskUpdated');
  }

  async function toggle(id, newState) {
    // Currently sends toggle to server and fetches everything again.
    // TODO: Update local todo and don't reload for instant change.
    // Is ok when everything on localhost tho.
    const resUpdate = await axios.put(global.baseUrl + '/tasks/' + id, {
      completed: newState
    });
    console.log(resUpdate);

    dispatch('taskUpdated');
  }
</script>

<div class="text-left text-lg flex my-2" class:checked="{task.completed}" on:mouseenter={toggleDel} on:mouseleave={toggleDel}>
  <div class="flex-none text-center w-8 mx-1" role="button" on:click="{() => toggle(task._id, !task.completed)}">
    {#if task.completed}
      <div class="items-center justify-center h-full flex"><i class="far fa-check-circle"></i></div>
    {:else}
      <div class="items-center justify-center h-full flex"><i class="far fa-circle"></i></div>
    {/if}
  </div>
  <div class="flex-grow" class:checked-title="{task.completed}" role="button" on:click="{() => toggle(task._id, !task.completed)}">
      { task.title }
  </div>
  {#if showDel}
    <span class="delete icon">
      <div on:click|once={deleteItem(task._id)}><i class="fas fa-trash"></i></div>
    </span>
  {/if}
</div>

<style lang="postcss">
  .checked {
    @apply text-gray-400;
  }

  .checked-title {
    @apply italic line-through;
  }
</style>
