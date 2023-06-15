<script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const nombre = ref('')
const descripcion = ref('')
const id = ref(0)

async function editarCategoria() {
  await http
    .patch(`${ENDPOINT}/${id}`, {
        nombre:nombre.value, 
        descripcion: descripcion.value
    })
    .then(() => router.push('/categorias'))
}

async function getCategoria() {
  await http.get(`${ENDPOINT}/${id}`).then((response) => {
    ;

     (nombre.value = response.data.nombre), 
     (descripcion.value = response.data.descripcion)
  })
}

function goBack() {
  router.go(-1)
}

onMounted(() => {
    id.value = Number(router.currentRoute.value.params.id)
  getCategoria()
})
</script>

<template>
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item">
          <RouterLink to="/categorias">Categorias</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page">Editar</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Editar Categoria</h2>
    </div>

    <div class="row">
      <form @submit.prevent="editarCategoria">

        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="nombre" placeholder="Nombre" required />
          <label for="nombre">Nombre</label>
        </div>
        
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="descripcion" placeholder="descripcion" required />
          <label for="descripcion">Descripcion</label>
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