<script>
  import { createEventDispatcher } from 'svelte';
  import { fade } from 'svelte/transition';
  import axios from 'axios';

  import global from '../global.js';

  export let task;
  export let editing;

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

<div class="flex py-1 my-1 text-lg text-left rounded-lg hover:shadow-md hover:bg-gray-50 dark:hover:bg-gray-800" class:checked="{task.completed}">
  <div class="flex-none w-8 mx-1 text-center" role="button" on:click="{() => toggle(task._id, !task.completed)}">
    {#if task.completed}
      <div class="flex items-center justify-center h-full"><i class="far fa-check-circle"></i></div>
    {:else}
      <div class="flex items-center justify-center h-full"><i class="far fa-circle"></i></div>
    {/if}
  </div>
  <div class="flex-grow" class:checked-title="{task.completed}" role="button" on:click="{() => toggle(task._id, !task.completed)}">
      { task.title }
  </div>
  {#if editing}
    <span class="flex-none w-8 mx-1 text-center text-red-500 rounded-lg shadow hover:text-red-800" transition:fade>
      <a role="button" on:click|once={deleteItem(task._id)}>
        <div class="flex items-center justify-center h-full"><i class="far fa-trash-alt"></i></div>
      </a>
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
