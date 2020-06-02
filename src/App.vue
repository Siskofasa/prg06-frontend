<template>
  <div id="app">
    <Index />
    <Create />
  </div>
</template>

<script>
import Index from './components/Index.vue'
import Create from './components/Create.vue'
import Edit from './components/Edit.vue'
import '../node_modules/bootstrap/dist/css/bootstrap.min.css';
import BootstrapVue from "bootstrap-vue"


export default {
  name: 'App',
  components: {
    Index,
    Edit,
    Create
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
  margin-left: auto;
}
</style>
