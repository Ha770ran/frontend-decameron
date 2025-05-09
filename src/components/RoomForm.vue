<template>
  <form @submit.prevent="registrarHabitacion" class="room-form">
    <h4>Registrar habitación para "{{ hotelName }}"</h4>

    <div class="form-group">
      <label>Tipo de habitación:</label>
      <select v-model="form.room_type" required>
        <option value="">--Selecciona--</option>
        <option value="STANDARD">STANDARD</option>
        <option value="JUNIOR">JUNIOR</option>
        <option value="SUITE">SUITE</option>
      </select>
    </div>

    <div class="form-group">
      <label>Acomodación:</label>
      <select v-model="form.accommodation" required>
        <option value="">--Selecciona--</option>
        <option value="SENCILLA">SENCILLA</option>
        <option value="DOBLE">DOBLE</option>
        <option value="TRIPLE">TRIPLE</option>
        <option value="CUÁDRUPLE">CUÁDRUPLE</option>
      </select>
    </div>

    <div class="form-group">
      <label>Cantidad:</label>
      <input type="number" v-model.number="form.quantity" required min="1" />
    </div>

    <button type="submit">Agregar Habitación</button>

    <p v-if="mensaje" :class="{ error: esError, success: !esError }">
      {{ mensaje }}
    </p>
  </form>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'


const props = defineProps({
  hotelId: Number,
  hotelName: String
  roomEditada: Object
})

const emit = defineEmits(['habitacion-creada'])

const api = import.meta.env.VITE_API_URL

const form = ref({
  room_type: '',
  accommodation: '',
  quantity: 1
})

const mensaje = ref('')
const esError = ref(false)

const registrarHabitacion = async () => {
  mensaje.value = ''
  esError.value = false

  if (!form.value.room_type || !form.value.accommodation || form.value.quantity <= 0) {
    esError.value = true
    mensaje.value = 'Todos los campos son obligatorios y la cantidad debe ser válida.'
    return
  }

  try {
  const data = { ...form.value, hotel_id: props.hotelId }

  let response
  if (form.value.id) {
    response = await axios.put(`${api}/rooms.php`, data)
  } else {
    response = await axios.post(`${api}/rooms.php`, data)
  }

  mensaje.value = response.data.message || 'Habitación guardada correctamente.'
  form.value = { room_type: '', accommodation: '', quantity: 1 }
  emit('habitacion-creada')
} catch (err) {
  esError.value = true
  mensaje.value = err.response?.data?.error || 'Error al guardar habitación.'
}

</script>

<style scoped>
.room-form {
  border: 1px solid #ccc;
  padding: 1rem;
  margin-top: 1rem;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  font-weight: bold;
}

input, select {
  width: 100%;
  padding: 0.4rem;
}

button {
  margin-top: 0.5rem;
  padding: 0.5rem 1rem;
}

.success {
  color: green;
}

.error {
  color: red;
}
</style>
