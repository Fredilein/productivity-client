<script> 
  import { onMount } from 'svelte';
  import axios from 'axios';

  import Slot from '../components/Slot.svelte';
  import NewSlot from '../components/NewSlot.svelte';

  // duplicate code...
  const getDays = (() => {
    return [
      { id: 0, name: 'Sunday', slots: [] },
      { id: 1, name: 'Monday', slots: [] },
      { id: 2, name: 'Tuesday', slots: [] },
      { id: 3, name: 'Wednesday', slots: [] },
      { id: 4, name: 'Thursday', slots: [] },
      { id: 5, name: 'Friday', slots: [] },
      { id: 6, name: 'Saturday', slots: [] }];
  });

  // TODO: Make config file duh
  const baseUrl = 'http://localhost:4040';

  let slots = [];
  let daysMapped = getDays();

  onMount(async () => {
    updateSlots();
  });

  async function updateSlots() {
    const res = await axios.get(baseUrl + '/slots');
    slots = res.data;
    updateMapping();
  }

  function updateMapping() {
    // This would be prettier with for...in but it didn't work
    daysMapped = getDays();
    for (var d = 0; d < daysMapped.length; d++) {
      for (var i = 0; i < slots.length; i++) {
        if (daysMapped[d].id == slots[i].day) {
          daysMapped[d].slots.push(slots[i]);
        }
      }
    }
  }

</script>

<main>
  <h1><i class="fas fa-calendar-week"></i> Week</h1>
  <p>Lists tasks for each weekday</p>
  <div class="container">
    {#each daysMapped as day}
      <h3>{day.name}</h3>
      {#if day.slots.length === 0}
        <p><i class="fas fa-check"></i></p>
      {:else}
        {#each day.slots as slot}
          <Slot {slot} on:taskUpdated={updateSlots} />
        {/each}
      {/if}
    {/each}
  </div>
  <NewSlot />
</main>
