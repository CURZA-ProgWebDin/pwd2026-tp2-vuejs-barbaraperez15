<script setup>
import { ref } from 'vue'

defineProps({
  categorias: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['crear-producto'])

const nombre = ref('')
const precio = ref('')
const stock = ref('')
const categoria = ref('')
const error = ref('')

function limpiarFormulario() {
  nombre.value = ''
  precio.value = ''
  stock.value = ''
  categoria.value = ''
}

function enviarFormulario() {
  error.value = ''

  if (!nombre.value.trim() || !precio.value || !stock.value || !categoria.value) {
    error.value = 'Todos los campos son obligatorios.'
    return
  }

  const precioNumerico = Number(precio.value)
  const stockNumerico = Number(stock.value)

  if (isNaN(precioNumerico) || precioNumerico <= 0) {
    error.value = 'El precio debe ser un numero valido.'
    return
  }

  if (isNaN(stockNumerico) || stockNumerico < 0) {
    error.value = 'El stock debe ser un numero valido.'
    return
  }

  emit('crear-producto', {
    nombre: nombre.value.trim(),
    precio: precioNumerico,
    stock: stockNumerico,
    categoria: categoria.value
  })

  limpiarFormulario()
}
</script>

<template>
  <form @submit.prevent="enviarFormulario">
    <h2>Crear producto</h2>

    <div>
      <label for="nombre">Nombre del producto</label>
      <input id="nombre" v-model="nombre" type="text" placeholder="Ej: Cuaderno" />
    </div>

    <div>
      <label for="precio">Precio</label>
      <input id="precio" v-model="precio" type="number" min="0" />
    </div>

    <div>
      <label for="stock">Stock</label>
      <input id="stock" v-model="stock" type="number" min="0" />
    </div>

    <div>
      <label for="categoria-producto">Categoria</label>
      <select id="categoria-producto" v-model="categoria">
        <option value="" disabled>Seleccionar categoria</option>
        <option v-for="item in categorias" :key="item" :value="item">
          {{ item }}
        </option>
      </select>
    </div>

    <p v-if="error" class="error">{{ error }}</p>

    <button type="submit">Guardar producto</button>
  </form>
</template>
