<template>
  <form @submit.prevent="submitForm" class="form-container">
    <h2>Registrar Hotel</h2>

    <div class="form-group">
      <label>Nombre:</label>
      <input v-model="form.name" required />
    </div>

    <div class="form-group">
      <label>Dirección:</label>
      <input v-model="form.address" required />
    </div>

    <div class="form-group">
      <label>Ciudad:</label>
      <input v-model="form.city" required />
    </div>

    <div class="form-group">
      <label>NIT:</label>
      <input
        v-model="form.nit"
        required
        pattern="^[0-9\-]+$"
        title="NIT válido, solo números y guiones"
      />
    </div>

    <div class="form-group">
      <label>Máx. habitaciones:</label>
      <input
        type="number"
        v-model.number="form.max_rooms"
        required
        min="1"
        title="Debe ser mayor a cero"
      />
    </div>


    <button type="submit">Crear Hotel</button>

    <p v-if="message" :class="{ error: error, success: !error }">
      {{ message }}
    </p>
  </form>
</template>

<script setup>

import { ref, watch, defineEmits } from 'vue'
import axios from 'axios'

const props = defineProps({
  hotelEditado: Object
})

const emit = defineEmits(['hotel-guardado'])

const api = import.meta.env.VITE_API_URL

const form = ref({
  id: null,
  name: '',
  address: '',
  city: '',
  nit: '',
  max_rooms: 1
})

const message = ref('')
const error = ref(false)

watch(() => props.hotelEditado, (nuevo) => {
  if (nuevo) {
    form.value = { ...nuevo }
  }
})

const submitForm = async () => {
  message.value = ''
  error.value = false

  if (!form.value.name || !form.value.nit || form.value.max_rooms <= 0) {
    error.value = true
    message.value = 'Todos los campos son obligatorios y válidos.'
    return
  }

  try {
    if (form.value.id) {
      // Editar
      const response = await axios.put(`${api}/hotels.php`, form.value)
      message.value = response.data.message || 'Hotel actualizado.'
    } else {
      // Crear nuevo
      const response = await axios.post(`${api}/hotels.php`, form.value)
      message.value = response.data.message || 'Hotel creado correctamente.'
    }

    emit('hotel-guardado')

    form.value = {
      id: null,
      name: '',
      address: '',
      city: '',
      nit: '',
      max_rooms: 1
    }
  } catch (err) {
    error.value = true
    message.value = err.response?.data?.error || 'Error al guardar hotel.'
  }
}
</script>

<style scoped>
.form-container {
  max-width: 500px;
  margin: 20px auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 0.5rem;
  margin-top: 0.25rem;
}

button {
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
}

.success {
  color: green;
  margin-top: 1rem;
}

.error {
  color: red;
  margin-top: 1rem;
}

@media (max-width: 600px) {
  .form-container {
    padding: 0.5rem;
  }

  input, button {
    font-size: 1rem;
  }

  table {
    font-size: 0.85rem;
  }
}
</style>
