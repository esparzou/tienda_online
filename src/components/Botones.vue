<template>
   <div class="container">
      <h1 class="title">Botones</h1>  
      <button class="button is-primary" 
              @click="vaciarCarrito">Vaciar Carrito
      </button>
      <button class="button is-info" 
              @click="recargarTienda">Recargar Tienda
      </button>
   </div>
</template>

<script>

   import { EventBus } from '../main.js';
   
   export default {

      methods: {
         /*Eventos emitidos por los botones y escuchados en
          los componentes Carrito y ListaProductos*/
         vaciarCarrito() {
            EventBus.$emit('delete');
         },
         recargarTienda() {
            EventBus.$emit('delete');
         }
      }
   }

</script>

<style scoped>

</style>