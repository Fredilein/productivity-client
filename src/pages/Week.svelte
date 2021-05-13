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

<div class="container p-3 mx-auto mb-12 font-mono text-left bg-gray-100 dark:bg-gray-900 dark:text-gray-100 shadow-md sm:rounded-lg">
  <div class="font-bold">Week</div>
  - Lists tasks for each weekday<br>
  - Add a new timeslot<br>
  - TODO: Form to add category<br>
  - Probably rework data model anyways<br>
</div>

<NewSlot on:slotAdded={updateSlots} />

{#each daysMapped as day}
  <div class="container flex items-center justify-center w-48 h-10 mx-auto text-2xl text-black bg-gray-100 rounded-full shadow-md dark:bg-gray-700 dark:text-gray-50">{day.name}</div>
  {#if day.slots.length === 0}
    <div class="mt-2 mb-4 dark:text-gray-50"><i class="fas fa-check"></i></div>
  {:else}
    {#each day.slots as slot}
      <Slot {slot} on:taskUpdated={updateSlots} />
    {/each}
  {/if}
{/each}
