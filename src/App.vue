<script setup>
import { ref, reactive, onMounted, watch } from 'vue';
import { db } from './data/guitarras';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';

const guitarras = ref([]);
const carrito = ref([]);
const guitarra = ref({});

onMounted(() => {
    guitarras.value = db
    guitarra.value = db[3]
    const carritoStorage = localStorage.getItem('carrito');
    if (carritoStorage) {
        carrito.value = JSON.parse(carritoStorage);
    }
});

watch(carrito, () => {
    guardarLocalStorage();
}, {
    deep:true
})

const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value));
}

const agregarCarrito = (guitarra) => {
    const existe = carrito.value.findIndex(producto => producto.id === guitarra.id);

    if (existe !== -1) {
        carrito.value[existe].cantidad++;
        return;
    }else{
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    };
}

const sumarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id);
    if (carrito.value[index].cantidad >= 5) return;
    carrito.value[index].cantidad++;
}

const restarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id);
    if (carrito.value[index].cantidad <= 1) return;
    carrito.value[index].cantidad--;
}

const eliminarProducto = (id) => {
    // Primera forma de eliminar un producto del carrito
    // const index = carrito.value.findIndex(producto => producto.id === id);
    // carrito.value.splice(index, 1);

    // Otra forma de eliminar un producto del carrito
    carrito.value = carrito.value.filter(producto => producto.id !== id);
}

const vaciarCarrito = () => {
    carrito.value = [];
}

</script>

<template>
    <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @agregar-carrito="agregarCarrito"
    @sumar-cantidad="sumarCantidad"
    @restar-cantidad="restarCantidad"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra
                v-for="guitarra in guitarras"
                :guitarra="guitarra"
                @agregar-carrito="agregarCarrito"
            />
        </div>
    </main>

    <Footer />
</template>
