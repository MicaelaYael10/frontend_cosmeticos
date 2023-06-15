<script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'
import type { Categoria } from '@/types/categoria';

const props = defineProps<{
    ENDPOINT_API: string;
    idProducto: number;
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
var categorias = ref<Categoria[]>([])

async function getCategorias() {
    categorias.value = await http.get(ENDPOINT).then((response) => response.data)
}

function toEdit(id: number) {
    router.push(`/categorias/editar/${id}`)
}

async function toDelete(id: number) {
    var r = confirm('¿Está seguro que se desea eliminar la Categoria?')
    if (r == true) {
        await http.delete(`${ENDPOINT}/${id}`).then(() => getCategorias())
    }
}

onMounted(() => {
    getCategorias()
})
</script>

<template>
    <div class="container">
        <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
                <li class="breadcrumb-item">
                    <RouterLink to="/">Inicio</RouterLink>
                </li>
                <li class="breadcrumb-item active" aria-current="page">Categorias</li>
            </ol>
        </nav>

        <div class="row">
            <h2>Lista de Categorias</h2>
            <div class="col-20">
                <RouterLink to="/categorias/crear">Crear Nuevo</RouterLink>
            </div>
        </div>
        <div class="table-responsive">
            <table class="table table-bordered table-striped table-hover">
                <thead>
                    <tr>
                        <th scope="col">N°</th>
                        <th scope="col">Id Producto</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">Descripcion</th>
                        <th scope="col">Acciones</th>
                    </tr>
                </thead>

            <tbody>
                    <tr v-for="(categoria, index) in categorias.values()" :key="categoria.id">

                        <th scope="row">{{ index + 1 }}</th>
                        <td>{{ categoria.idProducto }}</td>
                        <td>{{ categoria.nombre }}</td>
                        <td>{{ categoria.descripcion }}</td>
                        
                
                        <td>
                            <button class="btn btn-link" @click="toEdit(categoria.id)">Editar</button>
                            <button class="btn btn-link" @click="toDelete(categoria.id)">Eliminar</button>
                        </td>
                    </tr>
                </tbody>
             </table> 
         </div>
     </div> 
</template>

<style scoped></style>