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
</form>


<style>
  .newtask {
    padding: 12px;
  }

  .row-newtask {
    padding-left: 12px;
    padding-right: 12px;
  }

  .row-newtask-input {
    margin-bottom: 12px;
  }

  .col-select {
    padding: 0px;
    margin-right: 6px;
  }

  .col-submit {
    padding: 0px;
    margin-left: 6px;
  }

  .btn-newtask {
    width: 100%;
    height: 100%;
  }

  .input-task {
    width: 100%;
    height: 100%;
  }

  .theme {
    --borderRadius: 12px;
    border-radius: 12px;
  }
</style>
