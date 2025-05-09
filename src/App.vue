<template>
  <div class="main">
    <HotelForm @hotel-guardado="recargarHoteles" />
    <HotelList ref="listadoRef" />
  </div>
</template>

<script setup>
import HotelForm from './components/HotelForm.vue'
import HotelList from './components/HotelList.vue'
import { ref } from 'vue'

const listadoRef = ref(null)

const recargarHoteles = () => {
  listadoRef.value?.loadHotels()
}
</script>



