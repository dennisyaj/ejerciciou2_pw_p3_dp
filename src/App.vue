<template>

  <ListaEstados></ListaEstados>
</template>

<script>
import ListaEstados from './components/ListaEstados.vue';
export default {
  name: 'App',
  components: {
    ListaEstados
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
  margin-top: 60px;
}
</style>
