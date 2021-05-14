<script>
  import { Router, link } from 'svelte-routing';
</script>

<div class="container sticky top-0 justify-center h-12 mx-auto bg-indigo-500 shadow-xl max-w-screen-sm sm:top-8 sm:max-w-2xl dark:bg-indigo-800 sm:my-8 sm:rounded-lg">
  <div class="flex h-full text-lg text-center text-gray-50">
    <div class="btn-nav">
      <a href="/" use:link><i class="fas fa-home"></i> Home</a>
    </div>
    <div class="btn-nav">
      <a href="/today" use:link><i class="fas fa-calendar-day"></i> Today</a>
    </div>
    <div class="btn-nav">
      <a href="/week" use:link><i class="fas fa-calendar-week"></i> Week</a>
    </div>
  </div>
</div>

<style lang="postcss">
  .btn-nav {
    @apply flex-1 flex items-center justify-center hover:bg-indigo-600 hover:shadow m-1 sm:rounded-lg
  }
</style>
