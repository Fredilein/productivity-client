<script> 
  import { onMount } from 'svelte';
  import axios from 'axios';

  import Slot from '../components/Slot.svelte';
  import NewSlot from '../components/NewSlot.svelte';
  import global from '../global.js';

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

  let slots = [];
  let daysMapped = getDays();

  onMount(async () => {
    updateSlots();
  });

  async function updateSlots() {
    const res = await axios.get(global.baseUrl + '/slots');
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

<h1><i class="fas fa-calendar-week"></i> Week</h1>
<div class="container" style="margin: 20px;">
  <samp>- Lists tasks for each weekday</samp><br>
  <samp>- Add a new timeslot</samp><br>
  <samp>- TODO: Form to add category</samp><br>
</div>
<div class="container">
  {#each daysMapped as day}
    <div class="container mx-auto text-2xl bg-indigo-50 text-indigo-500 h-10 w-48 items-center justify-center flex rounded-full shadow-md">{day.name}</div>
    {#if day.slots.length === 0}
      <div class="mt-2 mb-4"><i class="fas fa-check"></i></div>
    {:else}
      {#each day.slots as slot}
        <Slot {slot} on:taskUpdated={updateSlots} />
      {/each}
    {/if}
  {/each}
</div>
<NewSlot on:slotAdded={updateSlots} />
