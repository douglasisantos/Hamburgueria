<template>
  <div id="nav">
    <RouterLink to="/" id="logo-url">
      <img :src="logo" :alt="alt" id="logo" />
    </RouterLink>
    <RouterLink to="/">Home</RouterLink>
    <RouterLink to="/pedidos">Pedidos</RouterLink>
  </div>
</template>

<script setup lang="ts">
import { RouterLink } from 'vue-router'

interface Props {
  logo: string
  alt: string
}

const props = defineProps<Props>()
</script>

<style>
#nav {
  background-color: #222;
  border-bottom: 4px solid #111;
  padding: 15px 50px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

 #nav #logo-url{
 margin:auto;
 margin-left: 0;
}
 #logo {
  width: 40px;
  height: 40px;
  margin-right: 20px;
}
#nav a {
  color: #fcba03;
  text-decoration: none;
  margin: 12px;
  font-size: 18px;
  transition: .5s;
}
#nav a:hover {
  color: #fff;
}
</style>
