<script>
  import { onMount } from 'svelte';
  import axios from 'axios';

  import NewTask from '../components/NewTask.svelte';
  import Slot from '../components/Slot.svelte';
  
  import global from '../global.js';

  let slots = [];

  onMount(async () => {
    updateSlots();
  });

  async function updateSlots() {
    const res = await axios.get(global.baseUrl + '/slots');
    slots = res.data;
    console.log(res);
  }
</script>

<div class="container p-3 mx-auto mb-12 font-mono text-left bg-gray-100 shadow-md dark:bg-gray-900 dark:text-gray-100 sm:rounded-lg">
  <div class="font-bold">Today</div>
  - Should list tasks for current day<br>
  - Actually lists basicly everything (all slots with all tasks)<br>
  - Can add a new task here<br>
</div>

<NewTask slots={slots} on:taskAdded={updateSlots}/>

{#if slots.length === 0}
  <p>Loading</p>
{:else}
  {#each slots as slot}
    <Slot {slot} on:taskUpdated={updateSlots} />
  {/each}
{/if}
