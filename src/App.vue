<!-- src\App.vue -->

<template>
  <div>
    <Header />
    <ProductsTable />
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import ProductsTable from './components/ProductsTable.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Header,
    ProductsTable,
    Footer
  }
}
</script>

<style>
@import './assets/main.css';
</style>