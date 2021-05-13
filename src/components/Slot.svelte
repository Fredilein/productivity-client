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

<div class="container mx-auto bg-white shadow-lg sm:rounded-lg p-3 m-12">
  <div class="h-12 flex bg-gray-100 shadow -mt-6 mb-3 rounded-lg">
    <div class="flex-1 items-center justify-start flex pl-6 font-bold text-lg">
      {slot.category.title}
    </div>
    {#if slot.start_time && slot.end_time}
      <div class="flex-1 items-center justify-end flex pr-6 font-mono text-lg">
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
