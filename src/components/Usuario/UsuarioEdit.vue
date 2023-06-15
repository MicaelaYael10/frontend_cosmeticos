 <script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const usuario = ref('')
const email = ref('')
const rol = ref('')
const premium = ref('')
const id = router.currentRoute.value.params['id']

async function editarUsuario() {
  await http
    .patch(`${ENDPOINT}/${id}`, {
        usuario: usuario.value,
        email: email.value,
        rol: rol.value,
        premium: premium.value
        
    })
    .then(() => router.push('/usuarios'))
}

async function getUsuario() {
  await http.get(`${ENDPOINT}/${id}`).then((response) => {
    ;

     (usuario.value = response.data.usuario), 
     (email.value = response.data.email), 
     (rol.value = response.data.rol), 
     (premium.value = response.data.premium)
  })
}

function goBack() {
  router.go(-1)
}

onMounted(() => {
  getUsuario()
})
</script>

<template>
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item">
          <RouterLink to="/usuarios">Usuarios</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Editar</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Editar Usuario</h2>
    </div>

    <div class="row">
      <form @submit.prevent="editarUsuario">

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="usuario" placeholder="usuario" required />
          <label for="nusuario">Usuario</label>
        </div>

      
         <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="email" placeholder="email" required />
          <label for="email">email</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="rol" placeholder="rol" required />
          <label for="rol">Rol</label>
        </div>

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="premium" placeholder="premium" required />
          <label for="premium">Premium</label>
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