<script>
  import { Router, link } from 'svelte-routing';
</script>

<div class="container sticky top-0 sm:top-8 mx-auto max-w-sm sm:max-w-2xl bg-indigo-500 dark:bg-indigo-800 shadow-xl sm:my-8 h-12 justify-center sm:rounded-lg">
  <div class="flex text-center text-lg h-full text-gray-50">
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
