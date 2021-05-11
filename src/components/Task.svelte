<script>
  export let task;
</script>

<div class="task" class:checked="{task.completed}" role="button" on:click>
  <span class="checkbox">
    {#if task.completed}
      <i class="far fa-check-circle"></i>
    {:else}
      <i class="far fa-circle"></i>
    {/if}
  </span>
  <span class="title">
    { task.title }
  </span>
</div>

<style>
  .task {
    height: 30px;
    font-size: 1.1em;
  }

  .checkbox {
    position: absolute;
    width: 50px;
    text-align: center;
    margin-top: 1px;
  }

  .title {
    padding-left: 50px;
  }

  .checked {
    color: gray;
  }
</style>
