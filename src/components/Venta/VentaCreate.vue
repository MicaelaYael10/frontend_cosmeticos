<script setup lang="ts">
import { ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  ENDPOINT_API: string
  idProducto: number
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const idProducto = ref('')
const cantidad = ref('')
const precio = ref('')
const descuento = ref('')

async function crearVenta() {
  await http
    .post(ENDPOINT, { idProducto: idProducto.value, cantidad:cantidad.value,precio: precio.value, descuento: descuento.value })
    .then(() => router.push('/categorias'))
}

function goBack() {
  router.go(-1)
}
</script>

<template>
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item">
          <RouterLink to="/ventas">Ventas</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Crear</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Crear Nueva Venta</h2>
    </div>

    <div class="row">
      <form @submit.prevent="crearVenta">

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="idProducto" placeholder="idProducto" required />
          <label for="idProducto">idProducto</label>
        </div>
        
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="cantidad" placeholder="cantidad" required />
          <label for="cantidad">Cantidad</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="precio" placeholder="precio" required />
          <label for="precio">precio</label>
        </div>
        
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="descuento" placeholder="cdescuento" required />
          <label for="descuento">Descuento</label>
        </div>

        <div class="text-center mt-3">
          <button type="submit" class="btn btn-primary btn-lg">Crear</button>
        </div>
      </form>
    </div>
    <div class="text-left">
      <button class="btn btn-link" @click="goBack">Volver</button>
    </div>
  </div>
</template>

<style></style>