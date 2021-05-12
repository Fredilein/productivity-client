<script>
  import { onMount, afterUpdate, createEventDispatcher } from 'svelte';
  import Select from 'svelte-select';
  import axios from 'axios';

  const dispatch = createEventDispatcher();

  // TODO: confiiiig
  const baseUrl = 'http://localhost:4040';

  let categories;
  let categoriesMap;
  let selectedDay;
  let selectedCat;

  // duplicate code...
  const days = [
      { value: 0, label: 'Sunday'},
      { value: 1, label: 'Monday'},
      { value: 2, label: 'Tuesday'},
      { value: 3, label: 'Wednesday'},
      { value: 4, label: 'Thursday'},
      { value: 5, label: 'Friday'},
      { value: 6, label: 'Saturday'}];

  onMount(() => {
    updateCategories();
  });

  async function updateCategories() {
    const res = await axios.get(baseUrl + '/categories');
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
    const res = await axios.post(baseUrl + '/slots', {
      category: selectedCat.value,
      day: selectedDay.value
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
      <div class="col col-select-mid">
        <Select items={days} on:select={handleSelectDay} placeholder="Select Day..."></Select>
      </div>
      <div class="col col-submit">
        <button class="btn btn-primary btn-new theme" disabled={!selectedDay || !selectedCat} type=submit><i class="fas fa-plus-circle"></i> Add</button>
      </div>
    </div>
  </div>
</form>


<style>
  .new {
    padding: 12px;
  }

  .row-new {
    padding-left: 12px;
    padding-right: 12px;
  }

  .col-select-left {
    padding: 0px;
    margin-right: 6px;
  }

  .col-select-mid {
    padding: 0px;
    margin-left: 6px;
    margin-right: 6px;
  }

  .col-submit {
    padding: 0px;
    margin-left: 6px;
  }

  .btn-new {
    width: 100%;
    height: 100%;
  }

  .theme {
    --borderRadius: 12px;
    border-radius: 12px;
  }
</style>
