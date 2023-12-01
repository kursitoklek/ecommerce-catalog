<template>
  <div class="app" id="app">
    <ProductDisplay/>
  </div>
</template>

<script>
import ProductDisplay from './components/ProductDisplay.vue'

export default {
  name: 'App',
  components: {
    ProductDisplay
  }
}
</script> 

<style>
html, body {
  width: 100%;
  height: 100%;
  padding: 0; 
  margin: 0;
 
}



</style>
