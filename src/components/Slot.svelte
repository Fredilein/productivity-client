<script>
  import { createEventDispatcher } from 'svelte';

  import Task from '../components/Task.svelte';

  export let slot;

  const dispatch = createEventDispatcher();

  let editing = false;

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

</script>

<div class="container p-3 m-12 mx-auto bg-white shadow-lg dark:bg-gray-900 dark:text-gray-50 sm:rounded-lg">
  <div class="flex h-12 mb-3 -mt-6 bg-gray-100 rounded-lg shadow dark:bg-gray-700">
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
</div>
