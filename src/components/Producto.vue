<template>
   <tr>
      <td>{{ producto.nombre }}</td>
      <td>{{ producto.id }}</td>
      <td>{{ producto.cat }}</td>
      <td>{{ producto.unidades }}</td>
      <td>{{ producto.precio }} €</td>
      <td>
         <form @submit.prevent="anadirCarrito">
            <input type="text" class="input" 
                   v-model.number="cantidad">
            <input type="submit"  class="button is-success" value="Añadir">
         </form>
      </td>
   </tr>
</template>

<script>
   
   import { EventBus } from '../main.js';
   
   export default {
      props:['producto'],
      data() {
         return {
            //Guarda el valor del input
            cantidad: '',
         }
      },
      methods: {
         //Añade un producto al carrito
         anadirCarrito() {
            //Compruba si la cantidad es un numero o mayor que 0
            if(!isNaN(this.cantidad) && this.cantidad > 0) {
               /*Emite el evento addCart al componente Producto,
               pasando un objeto con las propiedades del producto
               mas el valor del input*/
               EventBus.$emit('addCart', {
                  'nombre': this.producto.nombre, 
                  'id': this.producto.id, 
                  'cat': this.producto.cat, 
                  'precio': this.producto.precio, 
                  'cantidad': this.cantidad,
               });
               this.cantidad = '';

            } else {
               this.cantidad = '';
            }

         },
      }     

   }

</script>

<style>
   input[type="text"] {
      display: block;
      width: 4.5rem;
      margin-bottom: .5rem;
   }
</style>