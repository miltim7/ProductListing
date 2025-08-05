<!-- src\App.vue -->

<template>
  <div>
    <Header />
    <ProductsListing />
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import ProductsListing from './components/ProductsListing.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    Header,
    ProductsListing,
    Footer
  }
}
</script>

<style>
@import './assets/main.css';
</style>