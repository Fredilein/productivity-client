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

<!--
<form on:submit|preventDefault={handleSubmit}>
  <div class="container newtask theme">
    <div class="row row-newtask row-newtask-input">
      <input class="input-task theme" type="text" placeholder="New Task..." bind:value={title}>
    </div>
    <div class="row row-newtask">
      <div class="col col-select">
        <Select items={slotsMap} on:select={handleSelect} placeholder="Select Slot..."></Select>
      </div>
      <div class="col col-submit">
        <button class="btn btn-primary btn-newtask theme" disabled={!title || !selected} type=submit><i class="fas fa-plus-circle"></i> Add</button>
      </div>
    </div>
  </div>
</form> -->

<form on:submit|preventDefault={handleSubmit}>
  <div class="container mx-auto bg-gray-50 shadow-lg sm:rounded-lg p-3">

    <div class="relative flex w-auto flex-wrap items-stretch m-2">
      <span class="z-10 h-full leading-snug font-normal absolute text-center text-gray-800 absolute bg-transparent rounded text-base items-center justify-center w-8 pl-3 py-3">
        <i class="far fa-circle"></i>
      </span>
      <input type="text" placeholder="New Task..." class="px-3 py-3 placeholder-gray-300 text-gray-800 relative bg-white bg-white rounded-lg text-sm border-0 shadow-md outline-none focus:outline-none focus:ring w-full pl-10"/>
    </div>

    <div class="flex">
      <div class="flex-1 w-full m-2">
        <Select items={slotsMap} on:select={handleSelect} placeholder="Select Slot..." class="shadow rounded-lg"></Select>
      </div>
      <button class="bg-indigo-500 text-white active:bg-indigo-600 font-bold uppercase text-sm px-6 py-3 rounded-lg shadow hover:shadow-lg outline-none focus:outline-none m-2 ease-linear transition-all duration-150" type="button">
        <i class="fas fa-plus-circle"></i> Add
      </button>
    </div>
  </div>
</form>


<style>
  /* form { */
  /*   padding: 12px; */
  /* } */
  /* .newtask { */
  /*   padding: 12px; */
  /*   border: 1px solid rgba(0,0,0,.125); */
  /*   background-color: rgba(0,0,0,.03); */
  /*   box-shadow: 1px 2px 4px lightgrey; */
  /* } */

  /* .row-newtask { */
  /*   padding-left: 12px; */
  /*   padding-right: 12px; */
  /* } */

  /* .row-newtask-input { */
  /*   margin-bottom: 12px; */
  /* } */

  /* .col-select { */
  /*   padding: 0px; */
  /*   margin-right: 6px; */
  /* } */

  /* .col-submit { */
  /*   padding: 0px; */
  /*   margin-left: 6px; */
  /* } */

  /* .btn-newtask { */
  /*   width: 100%; */
  /*   height: 100%; */
  /* } */

  /* .input-task { */
  /*   width: 100%; */
  /*   height: 100%; */
  /* } */

  /* .theme { */
  /*   --borderRadius: 12px; */
  /*   border-radius: 12px; */
  /* } */
</style>
