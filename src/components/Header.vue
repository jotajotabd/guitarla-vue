<script setup>

import { computed } from 'vue';

const props = defineProps({
    carrito: {
        type: Array,
        required: true
    },
    guitarra: {
        type: Object,
        required: true
    }
});

defineEmits(['sumar-cantidad', 'restar-cantidad', 'agregar-carrito', 'eliminar-producto', 'vaciar-carrito']);

const totalAPagar = computed(() => {
    return props.carrito.reduce((total, item) =>  total + (item.cantidad * item.precio), 0)
})

</script>

<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="/img/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div
                        class="carrito"
                    >
                    <span v-if="carrito.length" class="alerta-carrito" type="button">{{ carrito.length }}</span>
                    <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />
                        <div>
                        </div>

                        <div id="carrito" class="bg-white p-3">
                            <p v-if="carrito.length === 0" class="text-center m-0">
                                El carrito esta vacio
                            </p>
                            <div v-else>
                                <table class="w-100 table">
                                <thead>
                                    <tr>
                                        <th>Imagen</th>
                                        <th>Nombre</th>
                                        <th>Precio</th>
                                        <th>Cantidad</th>
                                        <th></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="c in carrito">
                                        <td>
                                            <img class="img-fluid" :src="'/img/' + c.imagen + '.jpg'" alt="imagen guitarra">
                                        </td>
                                        <td>{{ c.nombre }}</td>
                                        <td class="fw-bold">
                                                {{ c.precio }}
                                        </td>
                                        <td class="flex align-items-start gap-4">
                                            <button
                                                type="button"
                                                class="btn btn-dark"
                                                @click="$emit('restar-cantidad',c.id)"
                                            >
                                                -
                                            </button>
                                                {{  c.cantidad }}
                                            <button
                                                type="button"
                                                class="btn btn-dark"
                                                @click="$emit('sumar-cantidad', c.id)"
                                            >
                                                +
                                            </button>
                                        </td>
                                        <td>
                                            <button
                                                class="btn btn-danger"
                                                type="button"
                                                @click="$emit('eliminar-producto', c.id)"
                                            >
                                                X
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>

                            <p class="text-end">Total pagar: <span class="fw-bold">${{ totalAPagar }}</span></p>
                            <button class="btn btn-dark w-100 mt-3 p-2"
                                    @click="$emit('vaciar-carrito')"
                                    >Vaciar Carrito
                            </button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo {{ guitarra.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">{{  guitarra.descripcion }}</p>
                    <p class="text-primary fs-1 fw-black">${{ guitarra.precio  }}</p>
                    <button
                        type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                        @click="$emit('agregar-carrito', guitarra)"
                    >Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
    </header>
</template>
