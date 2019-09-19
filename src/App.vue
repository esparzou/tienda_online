<template>
  <div id="app" class="container is-fluid">
    <lista-productos/>
    <carrito/>
    <botones/>
  </div>
</template>

<script>
  import ListaProductos from './components/ListaProductos.vue'
  import Carrito from './components/Carrito.vue'
  import Botones from './components/Botones.vue'
  

  export default {
    name: 'app',
    components: { ListaProductos, Carrito , Botones },
  }

</script>

<style>
#app {
  display: flex;
  margin-top: 5rem;
  margin-bottom: 5rem;
  text-align: center;
}
  
</style>
