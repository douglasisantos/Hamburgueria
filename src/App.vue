<template>
  <div class="layout-container">
    <NavBar :logo="logo_src" :alt="app_name" />
    <main class="page-container">
      <RouterView />
    </main>
    <Footer />
  </div>
</template>

<script setup lang="ts">
import NavBar from './components/NavBar.vue'
import Footer from './components/Footer.vue'
import { RouterView } from 'vue-router'

const app_name = 'Make Your Burger'
const logo_src = '/src/assets/logo.png'
</script>

<style scoped lang="scss">
/* Reset global */

/* Container geral da página */
.layout-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* Conteúdo principal deve crescer */
.page-container {
  flex: 1;
}



</style>
