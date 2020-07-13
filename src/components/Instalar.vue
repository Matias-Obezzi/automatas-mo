<template>
  <div class="text-light my-4">
    <p class="my-2 font-weight-bold h5">Instalá la app!</p>
    <div class="container rounded border border-light px-3 py-2 align-middle row mx-auto">
      <div class="col-md-8 col-12">
          <p>
              Podes utilizar la app desde el navegador web que prefieras tanto offline como online.
              <br>
              Se puede en PC como en celular.
          </p>
          <p>
              Beneficios de intalarla:
              <ul>
                  <li>Se puede usar offline y online</li>
                  <li>Saca los molestos marcos del navegador</li>
                  <li>La hace más fluida y eficiente</li>
                  <li>Se accede más fácilmente</li>
              </ul>
              Para instalarla tan solo tenes que hacer click en el siguiente boton
          </p>
          <button @click="install()" class="btn btn-primary my-auto align-middle px-5">
            Instalar
          </button>
      </div>
      <div class="col-12 col-md text-center align-middle my-auto">
          <i class="fas fa-download fa-7x my-auto text-light bg-primary p-5 rounded-circle border border-light d-none d-md-inline-block"></i>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  props:['deferredPrompt'],
  methods:{
    install(){
      // hide our user interface that shows our A2HS button
      this.show = false;
      // Show the prompt
      this.deferredPrompt.prompt();
      // Wait for the user to respond to the prompt
      this.deferredPrompt.userChoice.then((choiceResult) => {
        if (choiceResult.outcome === 'accepted') {
          console.log('User accepted the A2HS prompt');
        } else {
          console.log('User dismissed the A2HS prompt');
        }
        this.deferredPrompt = null;
      });
    }
  }
}
</script> 