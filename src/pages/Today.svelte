<script>
  import { onMount } from 'svelte';
  import axios from 'axios';

  import NewTask from '../components/NewTask.svelte';
  import Slot from '../components/Slot.svelte';

  // TODO: Make config file duh
  const baseUrl = 'http://localhost:4040';

  let slots = [];

  onMount(async () => {
    updateSlots();
  });

  async function updateSlots() {
    const res = await axios.get(baseUrl + '/slots');
    slots = res.data;
    console.log(res);
  }
</script>

<main>
  <h1><i class="fas fa-calendar-day"></i> Today</h1>
  <p>Lists tasks for today (actually all tasks rn...)</p>
  <NewTask slots={slots} on:taskAdded={updateSlots}/>
  <div class="container">
      {#if slots.length === 0}
        <p>Loading</p>
      {:else}
        {#each slots as slot}
          <Slot {slot} on:taskUpdated={updateSlots} />
        {/each}
      {/if}
  </div>
</main>
