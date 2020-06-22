<template>
  <div class="container">
    <nav class="navbar navbar-light bg-light">
      <a class="navbar-brand text-info" href="#">
        <i class="fas fa-film"></i>
        영화저장소
      </a>
      <ul class="nav justify-content-center">
        <li class="nav-item">
          <router-link class="nav-link text-info" :to="{ name: 'MovieView' }">Movie</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link text-info" :to="{ name: 'VideoView' }">Video</router-link>
        </li>
      </ul>
    </nav>
    <div class="container">
      <router-view/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
}
</script>

<style>
</style>
