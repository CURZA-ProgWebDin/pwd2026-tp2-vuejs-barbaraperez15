<script setup>
const props = defineProps({
  producto: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['eliminar-producto'])

function eliminar() {
  emit('eliminar-producto', props.producto.id)
}
</script>

<template>
  <tr>
    <td>{{ producto.nombre }}</td>
    <td>{{ producto.categoria }}</td>
    <td>${{ producto.precio }}</td>
    <td>{{ producto.stock }}</td>
    <td>${{ producto.precio * producto.stock }}</td>
    <td>
      <button type="button" @click="eliminar">
        Eliminar
      </button>
    </td>
  </tr>
</template>
