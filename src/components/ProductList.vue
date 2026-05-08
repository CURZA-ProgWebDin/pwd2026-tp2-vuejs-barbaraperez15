<script setup>
import ProductItem from './ProductItem.vue'

defineProps({
  productos: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['eliminar-producto'])

function eliminarProducto(id) {
  emit('eliminar-producto', id)
}
</script>

<template>
  <div>
    <h2>Productos</h2>

    <p v-if="productos.length === 0">
      No hay productos para mostrar.
    </p>

    <table v-else>
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Categoria</th>
          <th>Precio</th>
          <th>Stock</th>
          <th>Subtotal</th>
          <th>Acciones</th>
        </tr>
      </thead>

      <tbody>
        <ProductItem
          v-for="producto in productos"
          :key="producto.id"
          :producto="producto"
          @eliminar-producto="eliminarProducto"
        />
      </tbody>
    </table>
  </div>
</template>
