<template>
   <div class="container is-fluid">
      <h1 class="title">Listado de Productos</h1>
      <table class="table is-bordered is-striped">
         <tr>
            <th>Nombre</th>
            <th>Id</th>
            <th>Categoria</th>
            <th>Unidades</th>
            <th>Precio</th>
            <th></th>
         </tr>
         <producto v-for="(producto, index) in productosOrdenados"
                   :key="index"
                   :producto="producto"/>
      </table>
   </div>
</template>

<script>
   import { EventBus } from '../main.js';
   import axios from 'axios'
   import Producto from './Producto.vue'

   export default {
      components: {
         Producto,
      },
      data() {
         return {
            listadoProductos: [],
         }
      },
      computed: {
         //Ordena el listado de productos por la categoria
         productosOrdenados() {
            return this.listadoProductos.sort((a, b) => (a.cat > b.cat) ? 1 : ((b.cat > a.cat) ? -1 : 0));
         },

      },
      methods: {
         //Guarda la tienda en localstorage
         setTienda() {
            localStorage.setItem("tienda", JSON.stringify(this.listadoProductos));
         },
         //Obtiene la tienda de localstorage
         getTienda() {
            this.listadoProductos = JSON.parse(localStorage.getItem("tienda"));
            this.setTienda();
         },
         /*Hace una peticion ajax para pedir un listado de productos 
         y lo guarda en localstorage*/
         peticionTienda() {
            axios.get('https://my.api.mockaroo.com/productos.json?key=cfcc2dd0')
                 .then( response => {
                     this.listadoProductos = response.data;
                     this.setTienda();
            });
         }
      },
      mounted () {
         /*Comprueba si hay una tienda guarda en localstorage
         si no hay tienda se llama a la funcion para que pida una 
         si la hay se carga desde el localstorage*/
         if(!localStorage.getItem('tienda')) {
            this.peticionTienda(); 
         } else {
            this.getTienda();
         }
         /*Escucha el evento delete desde el componente Botones
         Borra la tienda de localstorage
         y vuele a hacer una peticion ajax para pedir una tienda nueva*/
         EventBus.$on('delete', () => {
            localStorage.removeItem('tienda');
            this.peticionTienda(); 
         });
    },
      
   
   }
</script>

<style>

</style>
