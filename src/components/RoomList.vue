<template>
  <div class="room-list" v-if="rooms.length">
    <h4>Habitaciones del hotel "{{ hotelName }}"</h4>
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Tipo</th>
          <th>Acomodación</th>
          <th>Cantidad</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="room in rooms" :key="room.id">
          <td>{{ room.id }}</td>
          <td>{{ room.room_type }}</td>
          <td>{{ room.accommodation }}</td>
          <td>{{ room.quantity }}</td>
          <td>
            <button @click="seleccionarParaEditar(room)">✏️</button>
            <button @click="eliminarHabitacion(room.id)">🗑️</button>
          </td>
        </tr>
      </tbody>
    </table>
    <RoomForm
      :hotelId="props.hotelId"
      :hotelName="props.hotelName"
      :roomEditada="habitacionEditando"
      @habitacion-creada="cargarHabitaciones"
     />

  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import axios from 'axios'
import RoomForm from './RoomForm.vue'


const props = defineProps({
  hotelId: Number,
  hotelName: String
})


const rooms = ref([])
const api = import.meta.env.VITE_API_URL
const habitacionEditando = ref(null)

const seleccionarParaEditar = (room) => {
  habitacionEditando.value = { ...room }
}

const eliminarHabitacion = async (id) => {
  if (!confirm('¿Deseas eliminar esta habitación?')) return

  try {
    await axios.delete(`${api}/rooms.php?id=${id}`)
    await cargarHabitaciones()
  } catch (error) {
    alert('Error al eliminar habitación.')
  }
}

const cargarHabitaciones = async () => {
  if (props.hotelId) {
    try {
      const response = await axios.get(`${api}/rooms.php?hotel_id=${props.hotelId}`)
      rooms.value = response.data
    } catch {
      rooms.value = []
    }
  }
}

watch(() => props.hotelId, () => {
  habitacionEditando.value = null
  cargarHabitaciones()
})

watch(() => props.hotelId, async (nuevoId) => {
  if (nuevoId) {
    try {
      const response = await axios.get(`${api}/rooms.php?hotel_id=${nuevoId}`)
      rooms.value = response.data
    } catch (error) {
      console.error('Error al cargar habitaciones:', error)
      rooms.value = []
    }
  }
})

watch(() => props.roomEditada, (nueva) => {
  if (nueva) {
    form.value = { ...nueva }
  }
})

</script>

<style scoped>
.room-list {
  margin-top: 1rem;
  padding: 1rem;
  border: 1px solid #ccc;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 0.4rem;
  border: 1px solid #ddd;
  text-align: left;
}
</style>
