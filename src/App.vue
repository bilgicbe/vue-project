<template>
    <div id="app">
        <h1 class="text-center text-2xl font-bold text-red-800 sm:pt-16 pt-8">Products</h1>
        <Products />
    </div>
</template>

<script>
import Products from "./components/Products.vue"
import './assets/tailwind.css'
import './assets/output.css'
export default {
    name: 'App',
    components: {
        Products
    },
    data() {
        return {
        }
    },
}

</script>

<style>
</style>