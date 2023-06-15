<script setup lang="ts">
import { ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const usuario = ref('')
const tipoPersona = ref('')
const nombre = ref('')
const apellido = ref('')
const direccion = ref('')
const email = ref('')



async function crearPersona() {
  await http
    .post(ENDPOINT, {usuario: usuario.value, tipoPersona: tipoPersona.value, nombre: nombre.value, apellido: apellido.value, direccion: direccion.value, email: email.value })
    .then(() => router.push('/personas'))
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
          <RouterLink to="/personas">Personas</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Crear</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Crear Nuevo Persona</h2>
    </div>

    <div class="row">
      <form @submit.prevent="crearPersona">
       
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="usuario" placeholder="usuario" required />
          <label for="usuario">Usuario</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="tipoPersona" placeholder="tipoPersona" required />
          <label for="tipoPersona">Tipo Persona</label>
        </div>

    
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="nombre" placeholder="pnombre" required />
          <label for="nombre">Nombre</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="apellido" placeholder="apellido" required />
          <label for="apellido">Apellido</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="direccion" placeholder="direccion" required />
          <label for="direccion">Direccion</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="email" placeholder="email" required />
          <label for="email">Email</label>
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