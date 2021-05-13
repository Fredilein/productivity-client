<script>
  import { createEventDispatcher } from 'svelte';

  import Task from '../components/Task.svelte';

  export let slot;

  const dispatch = createEventDispatcher();

  function prettyTime(start, end) {
    start.minute = (start.minute.toString().length === 2) ? start.minute : '0' + start.minute;
    end.minute = (end.minute.toString().length === 2) ? end.minute : '0' + end.minute;
    return start.hour + ':' + start.minute + ' - ' + end.hour + ':' + end.minute
  }

  function propagateTaskUpdated() {
    dispatch('taskUpdated');
  }

</script>

<div class="container p-3 m-12 mx-auto bg-white shadow-lg dark:bg-gray-900 dark:text-gray-50 sm:rounded-lg">
  <div class="flex h-12 mb-3 -mt-6 bg-gray-100 rounded-lg shadow dark:bg-gray-700">
    <div class="flex items-center justify-start flex-1 pl-6 text-lg font-bold">
      {slot.category.title}
    </div>
    {#if slot.start_time && slot.end_time}
      <div class="flex items-center justify-end flex-1 pr-6 font-mono text-lg">
        {prettyTime(slot.start_time, slot.end_time)}
      </div>
    {/if}
  </div>
  {#if slot.tasks.length === 0}
    <p class="no-tasks"><em>No tasks...</em></p>
  {:else}
    {#each slot.tasks as t}
      <Task task={t} on:taskUpdated={propagateTaskUpdated}/>
    {/each}
  {/if}
</div>
