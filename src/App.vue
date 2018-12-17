<template>
  <div id="app">
    <div class="row">
      <div class="col-md-12">
        <Add />
      </div>
    </div>
    
  </div>
</template>

<script>
import Add from './components/Add.vue';
export default {
  name: 'app',
  components: {
    Add
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
