<script>
  import { onMount, afterUpdate, createEventDispatcher } from 'svelte';
  import Select from 'svelte-select';
  import axios from 'axios';

  import global from '../global.js';

  const dispatch = createEventDispatcher();

  let categories;
  let categoriesMap;
  let selectedDay;
  let selectedCat;
  let start_time = {
    hour: null,
    minute: null
  };
  let end_time = {
    hour: null,
    minute: null
  };

  // Kinda works, kinda doesn't
  // time input is trash anyways
  let inputValid = false;
  $: if (selectedDay && selectedCat && start_time.hour < 24 && start_time.minute < 60
          && end_time.hour < 24 && end_time.minute < 60 
          && (start_time.hour < end_time.hour || (start_time.hour === end_time.hour && start_time.minute < end_time.minute))) {
    console.log('isvalid');
    inputValid = true;
  } else {
    inputValid = false;
  }

  onMount(() => {
    updateCategories();
  });

  async function updateCategories() {
    const res = await axios.get(global.baseUrl + '/categories');
    categories = res.data;
    console.log(res);
    categoriesMap = mapCategories();
  }

  function mapCategories() {
    return categories.map((cat) => {
      return {
        value: cat._id,
        label: cat.title
      }
    });
  }

  function handleSelectDay(event) {
    selectedDay = event.detail;
  }
  function handleSelectCat(event) {
    selectedCat = event.detail;
  }

  async function handleSubmit() {
    const res = await axios.post(global.baseUrl + '/slots', {
      category: selectedCat.value,
      day: selectedDay.value,
      start_time: start_time,
      end_time: end_time
    });
    dispatch('slotAdded', res.data);
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <div class="container p-3 mx-auto mb-8 shadow-lg bg-gray-50 sm:rounded-lg">

    <div class="container w-32 mx-auto text-white bg-indigo-500 rounded-full shadow-md -mt-7">New Slot</div>

    <div class="relative flex m-2">
      <div class="flex-1 p-1">
        <Select items={categoriesMap} on:select={handleSelectCat} placeholder="Select Category..."></Select>
      </div>
      <div class="flex-1 p-1">
        <Select items={global.days} on:select={handleSelectDay} placeholder="Select Day..."></Select>
      </div>
    </div>
    <div class="relative flex w-full m-2">
        <div class="flex-none">
          <input type="text" class="time-input" bind:value={start_time.hour}>
          <span>:</span>
          <input type="text" class="time-input" bind:value={start_time.minute}>
          <span>-</span>
          <input type="text" class="time-input" bind:value={end_time.hour}>
          <span>:</span>
          <input type="text" class="time-input" bind:value={end_time.minute}>
        </div>
        <button class="flex-1 mx-4 text-sm font-bold text-white uppercase bg-green-500 rounded-lg shadow outline-none acitve:bg-green-600 hover:shadow-lg focus:outline-none ease-linear transition-all duration-150" disabled={!inputValid} type=submit><i class="fas fa-plus-circle"></i> Add</button>
    </div>
  </div>
</form>

<style lang="postcss">
  .time-input {
    @apply w-8 p-2 ml-1 bg-white rounded text-sm border-0 shadow-md outline-none focus:outline-none focus:ring;
  }
</style>
