<script>
  import { onMount, afterUpdate, createEventDispatcher } from 'svelte';
  import Select from 'svelte-select';
  import axios from 'axios';

  import global from '../global.js';

  const dispatch = createEventDispatcher();

  export let slots = [];

  let selected;
  let title = '';
  let slotsMap;

  onMount(() => {
    slotsMap = mapSlots();
  });
  afterUpdate(() => {
    slotsMap = mapSlots();
  });

  function mapSlots() {
    return slots.map((slot) => {
      return {
        value: slot._id,
        label: '<strong>' + slot.category.title + '</strong> on ' + global.days.find(x => x.value === slot.day).label
      }
    });
  }

  function handleSelect(event) {
    selected = event.detail;
  }

  async function handleSubmit() {
    const res = await axios.post(global.baseUrl + '/tasks', {
      title: title,
      slot: selected.value
    });
    console.log(res);
    title = '';
    dispatch('taskAdded', res.data);
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <div class="container p-3 mx-auto shadow-lg bg-gray-100 sm:rounded-lg">

    <div class="container w-32 mx-auto text-white bg-indigo-500 rounded-full shadow-md -mt-7">New Task</div>

    <div class="relative flex flex-wrap items-stretch w-auto m-2">
      <span class="absolute z-10 items-center justify-center w-8 h-full py-3 pl-3 text-base font-normal leading-snug text-center text-gray-800 bg-transparent rounded">
        <i class="far fa-circle"></i>
      </span>
      <input type="text" placeholder="Enter Task..." class="relative w-full px-3 py-3 pl-10 text-sm text-gray-800 placeholder-gray-300 bg-white border-0 rounded-lg shadow-md outline-none focus:outline-none focus:ring" bind:value={title}/>
    </div>

    <div class="flex">
      <div class="flex-1 w-full m-2">
        <Select items={slotsMap} on:select={handleSelect} placeholder="Select Slot..." class="rounded-lg shadow"></Select>
      </div>
      <button class="px-6 py-3 m-2 text-sm font-bold text-white uppercase bg-green-500 rounded-lg shadow outline-none sm:w-48 active:bg-green-600 hover:shadow-lg focus:outline-none ease-linear transition-all duration-150" disabled={!title || !selected} type=submit>
        <i class="fas fa-plus-circle"></i> Add
      </button>
    </div>
  </div>
</form>
