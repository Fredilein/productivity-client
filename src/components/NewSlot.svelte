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

<h2><i class="fas fa-calendar-plus"></i> New Slot</h2>
<form on:submit|preventDefault={handleSubmit}>
  <div class="container new theme">
    <div class="row row-new">
      <div class="col col-select-left">
        <Select items={categoriesMap} on:select={handleSelectCat} placeholder="Select Category..."></Select>
      </div>
      <div class="col col-select-right">
        <Select items={global.days} on:select={handleSelectDay} placeholder="Select Day..."></Select>
      </div>
    </div>
    <div class="row row-new">
      <div class="col col-time">
        <input type="text" class="input-time" bind:value={start_time.hour}>
        <span>:</span>
        <input type="text" class="input-time" bind:value={start_time.minute}>
        <span>-</span>
        <input type="text" class="input-time" bind:value={end_time.hour}>
        <span>:</span>
        <input type="text" class="input-time" bind:value={end_time.minute}>
      </div>
      <div class="col col-submit">
        <button class="btn btn-primary btn-new theme" disabled={!inputValid} type=submit><i class="fas fa-plus-circle"></i> Add</button>
      </div>
    </div>
  </div>
</form>


<style>
  form {
    padding: 12px;
  }

  h2 {
    margin-top: 36px;
  }

  .new {
    padding: 12px;
    border: 1px solid rgba(0,0,0,.125);
    background-color: rgba(0,0,0,.03);
    box-shadow: 1px 2px 4px lightgrey;
  }

  .row-new {
    padding-left: 12px;
    padding-right: 12px;
    margin-bottom: 12px;
  }

  .col-select-left {
    padding: 0px;
    margin-right: 6px;
  }

  .col-select-right {
    padding: 0px;
    margin-left: 6px;
  }

  .col-submit {
    padding: 0px;
    margin-left: 6px;
  }

  .btn-new {
    width: 100%;
    height: 100%;
    padding-left: 3px;
    padding-right: 3px;
  }

  .theme {
    --borderRadius: 12px;
    border-radius: 12px;
  }

  .col-time {
    padding: 0px;
  }
  
  .input-time {
    border-radius: 12px;
    width: 48px;
  }
</style>
