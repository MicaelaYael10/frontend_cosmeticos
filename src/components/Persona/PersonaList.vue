<script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'
import type { Persona } from '@/types/persona';

const props = defineProps<{
    ENDPOINT_API: string;
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
var personas = ref<Persona[]>([])

async function getPersonas() {
    personas.value = await http.get(ENDPOINT).then((response) => response.data)
}

function toEdit(id: number) {
    router.push(`/personas/editar/${id}`)
}

async function toDelete(id: number) {
    var r = confirm('¿Está seguro que se desea eliminar el Persona?')
    if (r == true) {
        await http.delete(`${ENDPOINT}/${id}`).then(() => getPersonas())
    }
}

onMounted(() => {
    getPersonas()
})
</script>

<template>
    <div class="container">
        <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
                <li class="breadcrumb-item">
                    <RouterLink to="/">Inicio</RouterLink>
                </li>
                <li class="breadcrumb-item active" aria-current="page">Personas</li>
            </ol>
        </nav>

        <div class="row">
            <h2>Lista de Persona</h2>
            <div class="col-20">
                <RouterLink to="/personas/crear">Crear Nuevo</RouterLink>
            </div>
        </div>
        <div class="table">
            <table class="table table-bordered table-striped table-hover">
                <thead>
                    <tr>
                        <th scope="col">N°</th>
                        <th scope="col">Usuario</th>
                        <th scope="col">Tipo Persona</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">Apellido</th>
                        <th scope="col">Direccion</th>
                        <th scope="col">Acciones</th>
                    </tr>
                </thead>

            <tbody>
                    <tr v-for="(persona, index) in personas.values()" :key="persona.id">

                        <th scope="row">{{ index + 1 }}</th>

                        <td>{{ persona.usuario }}</td>
                        <td>{{ persona.tipoPersona }}</td>
                        <td>{{ persona.nombre }}</td>
                        <td>{{ persona.apellido }}</td>
                        <td>{{ persona.direccion }}</td>
                
                        <td>
                            <button class="btn btn-link" @click="toEdit(persona.id)">Editar</button>
                            <button class="btn btn-link" @click="toDelete(persona.id)">Eliminar</button>
                        </td>
                    </tr>
                </tbody>
             </table> 
         </div>
     </div> 
</template>

<style scoped></style>