
<script setup lang="ts">
import { onMounted, ref } from 'vue'
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
const id = router.currentRoute.value.params['id']

async function editarProducto() {
  await http
    .patch(`${ENDPOINT}/${id}`, {
        usuario: usuario.value,
        tipoPersona: tipoPersona.value,
        nombre: nombre.value,
        apellido: apellido.value,
        direccion: direccion.value,
        email: email.value
    })
    .then(() => router.push('/personas'))
}

async function getPersona() {
  await http.get(`${ENDPOINT}/${id}`).then((response) => {
    ;

     (usuario.value = response.data.usuario), 
     (tipoPersona.value = response.data.tipoPersona), 
     (nombre.value = response.data.nombre), 
     (apellido.value = response.data.apellido), 
     (direccion.value = response.data.direccion),
     (email.value = response.data.email)
  })
}

function goBack() {
  router.go(-1)
}

onMounted(() => {
  getPersona()
})
</script>

<template>
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item">
          <RouterLink to="/personas">Personas</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Editar</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Editar Persona</h2>
    </div>

    <div class="row">
      <form @submit.prevent="editarProducto">
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
          <button type="submit" class="btn btn-primary btn-lg">Guardar</button>
        </div>
      </form>
    </div>
    <div class="text-left">
      <button class="btn btn-link" @click="goBack">Volver</button>
    </div>
  </div>
</template>

<style></style>