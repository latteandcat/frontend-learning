<template>
  <div class="app">
    <app-header></app-header>
    <app-main></app-main>
    <app-footer></app-footer>
  </div>
</template>

<script>
import AppHeader from '@/components/Header.vue'
import AppMain from '@/components/Main.vue'
import AppFooter from '@/components/Footer.vue'
export default {
  name: 'App',
  components: {
    AppHeader,
    AppMain,
    AppFooter
  },
  provide() {
    return {
      title: 'list title provided by app'
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 500px;
  margin: 0 auto;
}
</style>
