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

<div class="card card-tasks">
  <div class="card-header">
    <span class="slot-title">
      <strong>
        {slot.category.title}
      </strong>
    </span>
    {#if slot.start_time && slot.end_time}
      <span class="slot-time">
        <em>
          {prettyTime(slot.start_time, slot.end_time)}
        </em>
      </span>
    {/if}
  </div>
  <div class="card-body">
    {#if slot.tasks.length === 0}
      <p class="no-tasks"><em>No tasks...</em></p>
    {:else}
      {#each slot.tasks as t}
        <Task task={t} on:taskUpdated={propagateTaskUpdated}/>
      {/each}
    {/if}
  </div>
</div>

<style>
  .card-body {
    text-align: left;
  }

  .card-tasks {
    margin-top: 12px;
    margin-bottom: 12px;
    border-radius: 12px;
  }

  .card-header {
    height: 41px;
  }

  .no-tasks {
    margin-bottom: 0px;
    text-align: center;
    color: grey;
  }
  
  .slot-title {
    font-size: 1.1em;
    position: absolute;
    left: 32px;
  }

  .slot-time {
    font-size: 1.1em;
    position: absolute;
    right: 32px;
  }
  
</style>
