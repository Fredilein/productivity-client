<script>
  import { createEventDispatcher } from 'svelte';
  import axios from 'axios';

  import Task from '../components/Task.svelte';
  import global from '../global.js';

  export let slot;

  const dispatch = createEventDispatcher();

  let editing = false;
  let newTask = false;
  let title = '';

  function prettyTime(start, end) {
    start.minute = (start.minute.toString().length === 2) ? start.minute : '0' + start.minute;
    end.minute = (end.minute.toString().length === 2) ? end.minute : '0' + end.minute;
    return start.hour + ':' + start.minute + ' - ' + end.hour + ':' + end.minute
  }

  function propagateTaskUpdated() {
    dispatch('taskUpdated');
  }

  function toggleEdit() {
    editing = !editing;
  }

  function toggleNewTask() {
    newTask = !newTask;
  }

  async function handleSubmit() {
    const res = await axios.post(global.baseUrl + '/tasks', {
      title: title,
      slot: slot._id
    });
    console.log(res);
    title = '';
    dispatch('taskUpdated', res.data);
  }

</script>

<div class="container relative p-3 m-12 mx-auto mb-16 bg-white shadow-lg dark:bg-gray-900 dark:text-gray-50 sm:rounded-lg">
  <div class="flex h-12 mx-2 mb-3 -mt-6 bg-gray-100 rounded-lg shadow dark:bg-gray-700">
    <div class="flex items-center justify-start flex-auto pl-6 text-lg font-bold">
      {slot.category.title}
    </div>
    {#if slot.start_time && slot.end_time}
      <div class="flex items-center justify-end flex-auto font-mono text-lg">
        {prettyTime(slot.start_time, slot.end_time)}
      </div>
    {/if}
    <a role="button" class="flex items-center justify-end flex-none px-3 m-1.5 ml-3 rounded-lg text-lg" class:shadow-inner={editing} on:click={toggleEdit}><i class="fas fa-pencil-alt"></i></a>
  </div>
  {#if slot.tasks.length === 0}
    <p class="no-tasks"><em>No tasks...</em></p>
  {:else}
    {#each slot.tasks as t}
      <Task task={t} {editing} on:taskUpdated={propagateTaskUpdated}/>
    {/each}
  {/if}
  {#if newTask}
    <form on:submit|preventDefault={handleSubmit}>
      <div class="flex mx-2 text-lg text-left">
        <div class="flex-none w-8 mx-1 text-center">
            <div class="flex items-center justify-center h-full"><i class="far fa-circle"></i></div>
        </div>
        <div class="flex-grow">
          <input type="text" placeholder="Enter Task..." class="relative w-full py-2 pl-2 text-lg text-gray-800 placeholder-gray-300 bg-white border-0 rounded-lg shadow-md outline-none focus:outline-none focus:ring" bind:value={title}/>
        </div>
      </div>
      <button class="bg-green-500 btn-new-task right-14" disabled={!title} type=submit>
        <i class="fas fa-check"></i>
      </button>
      <button class="bg-red-500 btn-new-task right-4" on:click={toggleNewTask}>
        <i class="fas fa-times"></i>
      </button>
    </form>
  {:else}
    <button class="bg-indigo-500 btn-new-task right-4" on:click={toggleNewTask}>
      <i class="fas fa-plus"></i>
    </button>
  {/if}
</div>

<style>
  .btn-new-task {
    @apply container absolute flex items-center justify-center w-8 h-8 mx-auto text-white rounded-full shadow-lg -bottom-6 -mt-7
  }
</style>
