<template>
  <div id="app" :class="{'bg-dark':$route.name!='Home'}" style="height:100vh;">
    <Navigation :key="$route.name"/>
    <router-view :class="{'container':$route.name!='Home'}" :showInstall="showInstall" :deferredPrompt="deferredPrompt" />
    <Footer class="mt-auto" />
  </div>
</template>

<script>
import Navigation from '@/components/Navigation.vue'
import Footer from '@/components/Footer.vue'
export default {
  components:{
    Navigation,
    Footer
  },
  data(){
    return{
      showInstall:false,
      deferredPrompt: undefined
    }
  },
  mounted(){
    window.addEventListener('beforeinstallprompt', (e) => {
      // Prevent Chrome 67 and earlier from automatically showing the prompt
      e.preventDefault();
      // Stash the event so it can be triggered later.
      this.deferredPrompt = e;
      console.log(e)
      // Update UI to notify the user they can add to home screen
      this.showInstall = true;
    })
  }
}
</script>

<style>
@import url('/fonts/TitilliumWeb-Regular.ttf');
*{
  font-family: 'Titillium Web', sans-serif;
}
html{
  scroll-behavior: smooth;
}
.link:hover{
  text-decoration: none;
}
</style>
