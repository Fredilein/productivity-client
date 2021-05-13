<script>
  import { createEventDispatcher } from 'svelte';
  import axios from 'axios';

  import global from '../global.js';

  export let task;

  const dispatch = createEventDispatcher();

  let showDel = false;

  function toggleDel() {
    showDel = !showDel;
  }

  async function deleteItem(id) {
    const resDel = await axios.delete(global.baseUrl + '/tasks/' + id);

    dispatch('taskUpdated');
  }

  async function toggle(id, newState) {
    // Currently sends toggle to server and fetches everything again.
    // TODO: Update local todo and don't reload for instant change.
    // Is ok when everything on localhost tho.
    const resUpdate = await axios.put(global.baseUrl + '/tasks/' + id, {
      completed: newState
    });
    console.log(resUpdate);

    dispatch('taskUpdated');
  }
</script>

<!--
<li class="list-group-item">
  <div class="task" class:checked="{task.completed}" on:mouseenter={toggleDel} on:mouseleave={toggleDel}>
    <span class="checkbox icon" role="button" on:click="{() => toggle(task._id, !task.completed)}">
      {#if task.completed}
        <i class="far fa-check-circle"></i>
      {:else}
        <i class="far fa-circle"></i>
      {/if}
    </span>
    <span class="title" role="button" on:click="{() => toggle(task._id, !task.completed)}">
      {#if task.completed}
        <em><s>{ task.title }</s></em>
      {:else}
        { task.title }
      {/if}
    </span>
    {#if showDel}
      <span class="delete icon">
        <div on:click|once={deleteItem(task._id)}><i class="fas fa-trash"></i></div>
      </span>
    {/if}
  </div>
</li>-->

<div class="text-left text-lg flex my-2" class:checked="{task.completed}" on:mouseenter={toggleDel} on:mouseleave={toggleDel}>
  <div class="flex-none text-center w-8 mx-1" role="button" on:click="{() => toggle(task._id, !task.completed)}">
    {#if task.completed}
      <div class="items-center justify-center h-full flex"><i class="far fa-check-circle"></i></div>
    {:else}
      <div class="items-center justify-center h-full flex"><i class="far fa-circle"></i></div>
    {/if}
  </div>
  <div class="flex-grow" class:checked-title="{task.completed}" role="button" on:click="{() => toggle(task._id, !task.completed)}">
      { task.title }
  </div>
  {#if showDel}
    <span class="delete icon">
      <div on:click|once={deleteItem(task._id)}><i class="fas fa-trash"></i></div>
    </span>
  {/if}
</div>

<style lang="postcss">
  .checked {
    @apply text-gray-400;
  }

  .checked-title {
    @apply italic line-through;
  }

  /* .task { */
  /*   font-size: 1.1em; */
  /*   border: 1px solid white; */
  /*   border-radius: 12px; */
  /* } */

  /* .bordered { */
  /*   border: 1px solid lightgrey; */
  /*   border-radius: 12px; */
  /* } */

  /* .icon { */
  /*   position: absolute; */
  /*   margin-top: 1px; */
  /*   top: 50%; */
  /*   -ms-transform: translateY(-50%); */
  /*   transform: translateY(-50%); */
  /* } */

  /* .checkbox { */
  /*   width: 50px; */
  /*   text-align: center; */
  /* } */

  /* .title { */
  /*   padding-left: 50px; */
  /*   display: inline-block; */
  /*   max-width: 90%; */
  /*   line-height: 1.2; */
  /* } */

  /* .checked { */
  /*   color: grey; */
  /* } */

  /* .delete { */
  /*   color: lightgrey; */
  /*   position: absolute; */
  /*   right: 16px; */
  /* } */

  /* .list-group-item { */
  /*   border: 0; */
  /*   padding-top: 3px; */
  /*   padding-bottom: 3px; */
  /*   padding-left: 0px; */
  /*   padding-right: 0px; */
  /* } */
</style>
