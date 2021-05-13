<script>
  import Router from './routes/index.svelte';
  import Navbar from './components/Navbar.svelte';
</script>


<Navbar />
<div class="container mx-auto max-w-sm md:max-w-2xl text-center">
  <Router />
</div>

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  @layer base {
    h1 {
      @apply text-3xl;
    }
  }
</style>
