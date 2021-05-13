<script>
  import { Router, link } from 'svelte-routing';
</script>

<nav class="navbar fixed-top navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#"><i class="fas fa-bed"></i> Productivity</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <Router>
        <ul class="navbar-nav">
          <li class="nav-item">
            <a class="nav-link" href="/" use:link><i class="fas fa-home"></i> Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="/today" use:link><i class="fas fa-calendar-day"></i> Today</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="/week" use:link><i class="fas fa-calendar-week"></i> Week</a>
          </li>
        </ul>
      </Router>
    <div>
  </div>
</nav>

<style>
  a {
    text-decoration: none;
  }
</style>
