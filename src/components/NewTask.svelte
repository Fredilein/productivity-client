<script>
  import { onMount, createEventDispatcher } from 'svelte';
  import axios from 'axios';

  const dispatch = createEventDispatcher();

  // TODO: confiiiig
  const baseUrl = 'http://localhost:4040';

  let slots = [];
  let selected;
  let title = '';

  onMount(async () => {
    const res = await axios.get(baseUrl + '/slots');
    slots = res.data;
    console.log(res);
  });

  async function handleSubmit() {
    const res = await axios.post(baseUrl + '/tasks', {
      title: title,
      slot: selected._id
    });
    console.log(res);
    dispatch("taskAdded", res.data);
  }
</script>

<p>selected slot {selected ? selected._id : '[waiting...]'}</p>

<form on:submit|preventDefault={handleSubmit}>
  <input type="text" bind:value={title}>
  <select bind:value={selected}>
    {#each slots as slot}
      <option value={slot}>
        Category: {slot.category.title}, Day: {slot.day}
      </option>
    {/each}
  </select>

  <button class="btn btn-primary" disabled={!title} type=submit>Add</button>
</form>
