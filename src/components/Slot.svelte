<script>
  import { createEventDispatcher } from 'svelte';
  import axios from 'axios';

  import Task from '../components/Task.svelte';

  export let slot;

  const dispatch = createEventDispatcher();

  // TODO: Make config file
  const baseUrl = 'http://localhost:4040';

  async function toggle(id, newState) {
    // Currently sends toggle to server and fetches everything again.
    // TODO: Update local todo and don't reload for instant change.
    // Is ok when everything on localhost tho.
    const resUpdate = await axios.put(baseUrl + '/tasks/' + id, {
      completed: newState
    });
    console.log(resUpdate);

    dispatch('taskUpdated');
  }
</script>

<div class="card card-tasks">
  <div class="card-header">
    {slot.category.title}
  </div>
  <div class="card-body">
    <ul class="list-group">
      {#each slot.tasks as t}
        <Task task={t} on:click="{() => toggle(t._id, !t.completed)}"/>
      {/each}
    </ul>
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
</style>
