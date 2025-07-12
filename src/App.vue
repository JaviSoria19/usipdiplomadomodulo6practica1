<template>
  <div class="app-container">
    <HomeView />
    <footer>Diplomado USIP Fullstack V7 - Módulo 6 Práctica 1.<br>
      Diplomantes: (integrantes.txt).<br>
      Tecnologías utlizadas: Vue.js, Axios, JSON Server.
    </footer>
  </div>
</template>

<script>
import HomeView from './views/HomeView.vue'
export default {
  components: { HomeView }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: "Segoe UI", Roboto, sans-serif;
  background-color: #f5f7fa;
}

.app-container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem;
}
</style>
