<template>
   <div class="container is-fluid">
      <h1 class="title">Carrito</h1>
      <table class="table is-bordered is-striped">
         <tr>
            <th>Nombre</th>
            <th>Id</th>
            <th>Categoria</th>
            <th>Precio</th>
            <th>Unidades</th>
         </tr>
         <tr v-for="(articulo, index) in carrito" :key="index">
            <td>{{ articulo.nombre }}</td>
            <td>{{ articulo.id }}</td>
            <td>{{ articulo.cat }}</td>
            <td>{{ articulo.precio }} €</td>
            <td>{{ articulo.cantidad }}</td>
         </tr>
      </table>
   </div>
</template>

<script>

   import { EventBus } from '../main.js';

   export default {
      data() {
         return {
            carrito: [],
         }
      },
      methods: {
         //Guarda la tienda en localstorage
         setCarrito() {
            localStorage.setItem("carrito", JSON.stringify(this.carrito));
         },
         /*Carga la tienda en localstorage y 
         la vuelve a guardar en localstorage*/
         getCarrito() {
            this.carrito = JSON.parse(localStorage.getItem("carrito"));
            this.setCarrito();
         },
      },   
      mounted() {
         /*Cuando se monta el componente carga el carrito de localstorage
         si ya existe en localstorage*/
         if(localStorage.getItem('carrito')) {
            this.getCarrito();
         } 

            
         /*Escucha el evento addCart emitido por el componente Producto
         y le pasa un producto*/
         EventBus.$on('addCart', (producto) => {
            //Comprueba si el producto recibido ya existe en el carrito
            const productInCart = this.carrito.find( (item) => item.nombre === producto.nombre);
            //Si no existe se añade al carrito
            if(!productInCart) {              
               this.carrito.push(producto);
            //Si existe, se suma la cantidad al producto
            } else {
               productInCart.cantidad += producto.cantidad;
            }
            //Guarda el carrito en localStorage
            this.setCarrito();
         })
         /*Escucha el evento delete emitido por Botones y 
         vacia el carrito y elemina el carrito del localstorage*/
         EventBus.$on('delete', () => {
            this.carrito = [];
            localStorage.removeItem('carrito');
         });
      },
   }
</script>

<style>

</style>