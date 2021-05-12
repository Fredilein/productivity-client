<script>
  import { onMount } from 'svelte';
  import axios from 'axios';

  import Task from '../components/Task.svelte';
  import NewTask from '../components/NewTask.svelte';

  // TODO: Make config file duh
  const baseUrl = 'http://localhost:4040';

  let slots = [];

  onMount(async () => {
    const res = await axios.get(baseUrl + '/slots');
    slots = res.data;
    console.log(res);
  });

  async function updateSlots() {
    const res = await axios.get(baseUrl + '/slots');
    slots = res.data;
    console.log(res);
  }

  async function toggle(id, newState) {
    // Currently sends toggle to server and fetches everything again.
    // TODO: Update local todo and don't reload for instant change.
    // Is ok when everything on localhost tho.
    const resUpdate = await axios.put(baseUrl + '/tasks/' + id, {
      completed: newState
    });
    console.log(resUpdate);

    const res = await axios.get(baseUrl + '/slots');
    slots = res.data;
    console.log(res);
  }
</script>

<main>
  <h1>Today</h1>
  <p>Lists tasks for today</p>
  <NewTask on:taskAdded={updateSlots}/>
  <div class="container">
      {#if slots.length === 0}
        <p>Loading</p>
      {:else}
        {#each slots as slot}
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
        {/each}
      {/if}
  </div>
</main>

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
