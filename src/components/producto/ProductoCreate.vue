<script setup lang="ts">
import { ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const nombreArticulo = ref('')
const codigo = ref('')
const marca = ref('')
const precio = ref('')
const descripcion = ref('')

async function crearProducto() {
  await http
    .post(ENDPOINT, { codigo: codigo.value, nombreArticulo: nombreArticulo.value, marca: marca.value, precio:precio.value, descripcion: descripcion.value })
    .then(() => router.push('/productos'))
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
          <RouterLink to="/productos">Productos</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Crear</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Crear Nuevo Producto</h2>
    </div>

    <div class="row">
      <form @submit.prevent="crearProducto">
       
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="codigo" placeholder="codigo" required />
          <label for="codigo">Codigo del Articulo</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="nombreArticulo" placeholder="Nombre del Articulo" required />
          <label for="nombreArticulo">Nombre del Articulo</label>
        </div>

        <div class="form-floating">
            <input
            type="text"
            class="form-control"
            v-model="marca"
            placeholder="Marca del Producto"
            required/>
            <label for="marca">Marca</label>
         </div>
    
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="precio" placeholder="precio" required />
          <label for="precio">Precio del Producto</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="descripcion" placeholder="descripcion" required />
          <label for="descripcion">Descripcion</label>
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