<script>
  import { Router, link } from 'svelte-routing';
</script>

<main>
	<h1>Productivity</h1>
	<p>Name recommendations are appreciated...</p>
        <Router>
          <a href="/today" use:link>Today</a>
          <a href="/week" use:link>Week</a>
        </Router>
</main>

<style>
</style>
