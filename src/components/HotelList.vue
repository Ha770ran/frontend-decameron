<template>
  <div class="hotel-list">
    <h2>Listado de Hoteles</h2>

    <table v-if="hotels.length">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nombre</th>
          <th>Dirección</th>
          <th>Ciudad</th>
          <th>NIT</th>
          <th>Máx. habitaciones</th>
          <th>Acciones</th> 
        </tr>
      </thead>
      <tbody>
        <tr v-for="hotel in hotels" :key="hotel.id">
          <td>{{ hotel.id }}</td>
          <td>{{ hotel.name }}</td>
          <td>{{ hotel.address }}</td>
          <td>{{ hotel.city }}</td>
          <td>{{ hotel.nit }}</td>
          <td>{{ hotel.max_rooms }}</td>
          <td>
            <button @click="$emit('editar', hotel)">✏️</button>
            <button @click="eliminarHotel(hotel.id)">🗑️</button>
            <button @click="hotelSeleccionado = hotel">Ver habitaciones</button>   
          </td>
        </tr>
      </tbody>
    </table>
    <RoomList
      v-if="hotelSeleccionado"
      :hotelId="hotelSeleccionado.id"
      :hotelName="hotelSeleccionado.name"
    />
    <RoomForm
     v-if="hotelSeleccionado"
     :hotelId="hotelSeleccionado.id"
     :hotelName="hotelSeleccionado.name"
     @habitacion-creada="loadRooms"
    />
    <p v-else>Cargando hoteles o no hay registros...</p>
  </div>
</template>


<script setup>
import RoomForm from './RoomForm.vue'
import RoomList from './RoomList.vue'
import { ref, onMounted, defineExpose } from 'vue'
import axios from 'axios'

const hotelSeleccionado = ref(null)
const hotels = ref([])
const api = import.meta.env.VITE_API_URL

const loadHotels = async () => {
  try {
    const response = await axios.get(`${api}/hotels.php`)
    hotels.value = response.data
  } catch (error) {
    console.error('Error cargando hoteles:', error)
  }
}

const eliminarHotel = async (id) => {
  if (!confirm('¿Deseas eliminar este hotel?')) return

  try {
    await axios.delete(`${api}/hotels.php?id=${id}`)
    alert('Hotel eliminado correctamente')
    await loadHotels()
  } catch (error) {
    alert(error.response?.data?.error || 'Error al eliminar hotel')
  }
}

const editarHotel = (hotel) => {
  alert(`(Próximamente) Editar hotel:\n${hotel.name}`)
  // Aquí puedes abrir un modal o enviar el objeto a un formulario externo
}

onMounted(loadHotels)
defineExpose({ loadHotels })

onMounted(async () => {
  try {
    const response = await axios.get(`${api}/hotels.php`)
    hotels.value = response.data
  } catch (error) {
    console.error('Error cargando hoteles:', error)
  }
})
</script>

<style scoped>

.hotel-list {
  max-width: 900px;
  margin: 20px auto;
  padding: 1rem;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background-color: #f0f0f0;
}

th, td {
  padding: 0.5rem;
  border: 1px solid #ccc;
  text-align: left;
}

@media (max-width: 600px) {
  table {
    font-size: 0.85rem;
    display: block;
    overflow-x: auto;
  }
}

button {
  margin-right: 5px;
  padding: 5px 10px;
  font-size: 0.9rem;
  border-radius: 5px;
  border: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}

</style>

