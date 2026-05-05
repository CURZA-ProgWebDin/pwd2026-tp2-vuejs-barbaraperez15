<script setup>
import { computed, reactive, ref } from 'vue'
import ProductForm from './components/ProductForm.vue'
import ProductList from './components/ProductList.vue'

const categorias = ['Alimentos', 'Limpieza', 'Tecnologia', 'Libreria']

const productos = reactive([])
const filtroCategoria = ref('Todos')
let proximoId = 1

function crearProducto(producto) {
  productos.push({
    id: proximoId,
    nombre: producto.nombre,
    precio: producto.precio,
    stock: producto.stock,
    categoria: producto.categoria
  })

  proximoId++
}

function eliminarProducto(id) {
  const indice = productos.findIndex((producto) => producto.id === id)

  if (indice !== -1) {
    productos.splice(indice, 1)
  }
}

const productosFiltrados = computed(() => {
  if (filtroCategoria.value === 'Todos') {
    return productos
  }

  return productos.filter((producto) => producto.categoria === filtroCategoria.value)
})

const cantidadTotal = computed(() => productos.length)

const valorTotalInventario = computed(() =>
  productos.reduce((total, producto) => total + producto.precio * producto.stock, 0)
)
</script>

<template>
  <main>
    <h1>Gestion de productos</h1>

    <ProductForm :categorias="categorias" @crear-producto="crearProducto" />

    <hr>

    <h2>Resumen</h2>
    <p>Cantidad total de productos: {{ cantidadTotal }}</p>
    <p>Valor total del inventario: ${{ valorTotalInventario }}</p>

    <hr>

    <label for="categoria">Filtrar por categoria: </label>
    <select id="categoria" v-model="filtroCategoria">
      <option>Todos</option>
      <option v-for="categoria in categorias" :key="categoria">
        {{ categoria }}
      </option>
    </select>

    <ProductList :productos="productosFiltrados" @eliminar-producto="eliminarProducto" />
  </main>
</template>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 20px;
}

input,
select,
button {
  margin: 5px;
}

.error {
  color: red;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  border: 1px solid #ccc;
  padding: 8px;
}
</style>
