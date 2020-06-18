<template>
  <div id="app" style="height:100vh">
    <h3 class="text-center py-2 bg-dark text-light mb-3">Reconocedor de palabras en autómatas</h3>
    <router-view class="bg-light py-2 rounded"/>
    <Footer class="mt-auto" />
  </div>
</template>

<script>
import Footer from '@/components/Footer.vue'
export default {
  components:{
    Footer
  }
}
</script>

<style>
</style>
