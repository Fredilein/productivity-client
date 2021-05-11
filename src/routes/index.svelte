<script>
  import { Router, Route } from 'svelte-routing';
  import Home from '../pages/Home.svelte';
  import Today from '../pages/Today.svelte';
  export let url = '';
</script>

<Router {url}>
  <div>
    <Route path="today" component={Today} />
    <Route path="/" component={Home} />
  </div>
</Router>
