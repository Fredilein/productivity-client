<script>
  import { onMount, createEventDispatcher } from 'svelte';
  import Select from 'svelte-select';
  import axios from 'axios';

  const dispatch = createEventDispatcher();

  // TODO: confiiiig
  const baseUrl = 'http://localhost:4040';

  let slots = [];
  let slotsMap;
  let selected;
  let title = '';

  onMount(async () => {
    const res = await axios.get(baseUrl + '/slots');
    slots = res.data;
    console.log(res);

    slotsMap = slots.map((slot) => {
      return {
        value: slot._id,
        label: slot.category.title + ', day ' + slot.day
      }
    });
  });


  function handleSelect(event) {
    selected = event.detail;
  }

  async function handleSubmit() {
    const res = await axios.post(baseUrl + '/tasks', {
      title: title,
      slot: selected.value
    });
    console.log(res);
    title = '';
    dispatch("taskAdded", res.data);
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
