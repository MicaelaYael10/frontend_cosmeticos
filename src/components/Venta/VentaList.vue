<script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'
import type { Venta } from '@/types/venta';


const props = defineProps<{
    ENDPOINT_API: string;
    idProducto: number;
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
var ventas = ref<Venta[]>([])

async function getVentas() {
    ventas.value = await http.get(ENDPOINT).then((response) => response.data)
}

function toEdit(id: number) {
    router.push(`/ventas/editar/${id}`)
}

async function toDelete(id: number) {
    var r = confirm('¿Está seguro que se desea eliminar la Venta?')
    if (r == true) {
        await http.delete(`${ENDPOINT}/${id}`).then(() => getVentas())
    }
}

onMounted(() => {
    getVentas()
})
</script>

<template>
    <div class="container">
        <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
                <li class="breadcrumb-item">
                    <RouterLink to="/">Inicio</RouterLink>
                </li>
                <li class="breadcrumb-item active" aria-current="page">Ventas</li>
            </ol>
        </nav>

        <div class="row">
            <h2>Lista de Ventas</h2>
            <div class="col-20">
                <RouterLink to="/ventas/crear">Crear Nuevo</RouterLink>
            </div>
        </div>
        <div class="table-responsive">
            <table class="table table-bordered table-striped table-hover">
                <thead>
                    <tr>
                        <th scope="col">N°</th>
                        <th scope="col">idProducto</th>
                        <th scope="col">Cantidad</th>
                        <th scope="col">Precio</th>
                        <th scope="col">Acciones</th>
                    </tr>
                </thead>

            <tbody>
                    <tr v-for="(venta, index) in ventas.values()" :key="venta.id">

                        <th scope="row">{{ index + 1 }}</th>
                        <td>{{ venta.idProducto }}</td>
                        <td>{{ venta.cantidad }}</td>
                        <td>{{ venta.precio }}</td>
                        <td>{{ venta.descuento }}</td>
                        <td>
                            <button class="btn btn-link" @click="toEdit(venta.id)">Editar</button>
                            <button class="btn btn-link" @click="toDelete(venta.id)">Eliminar</button>
                        </td>
                    </tr>
                </tbody>
             </table> 
         </div>
     </div> 
</template>

<style scoped></style>