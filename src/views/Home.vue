<template>
  <div class="bg-light">
    <div class="banner position-relative mx-auto w-100 my-auto align-middle text-center" style="padding-top:10rem;">
      <div class="text-center text-light font-titillium-web mb-2" :class="{'display-4':!mobile, 'h1':mobile}">
        Tenes un autómata y necesitas <br> saber si reconoce una palabra?
      </div>
      <div class="text-center text-light font-weight-bold mx-lg-auto d-inline-block bg-light rounded p-2 mx-3" :class="{'display-4':!mobile, 'h1':mobile}">
        <p class="rounded mx-auto d-inline-block rta my-0 pb-1 ">Estás en el lugar correcto</p>
      </div>
      <div class="scroll-icon">
        <a href="#herramientas">
          <i class="fas fa-chevron-circle-up fa-2x text-white"></i>
        </a>
      </div>
    </div>
    <div class="container" :class="{'p-herramientas':$route.hash=='#herramientas'}" id="herramientas">
      <h3>Herramientas de la app</h3>
      <div class="row mx-auto pt-3">
        <div class="col-12 col-lg">
          <ul class="px-2 mx-3">
            <li>Ingresar autómatas:
              <ul>
                <li>Finitos</li>
                <li>Pila</li>
                <li>Turing</li>
              </ul>
            </li>
            <li>Ver digrafo del autómata</li>
            <li>Saber si una palabra es reconocida o no por el autómata</li>
          </ul>
        </div>
        <div class="col-12 col-lg my-auto">
          <img src="/img/fms.png" class="img-fluid d-block mx-auto rounded" style="max-height:200px;">
        </div>
      </div>
    </div>
    <div class="container mt-3" id=galeria>
      <h3>Galería</h3>
      <div class="row mx-auto"> 
        <div class="col-12 col-lg-8 row mx-0 my-2 pr-lg-1">
          <div class="col px-0 mr-1 my-auto">
            <img src="/img/fms-4.png" class="img-fluid mx-auto rounded border p-1 mb-2 bg-white" alt="">
            <img src="/img/fms-5.png" class="img-fluid mx-auto rounded border p-1 bg-white" alt="">
            <img src="/img/fms-6.png" class="img-fluid mx-auto rounded border p-1 mt-2 bg-white d-none d-lg-inline-block" alt="">
          </div>
          <div class="col-8 px-0 ml-1 my-auto">
            <img src="/img/fms-2.png" class="img-fluid mx-auto rounded border p-1" alt="">
          </div>
        </div>
        <div class="col-12 col-lg-4 mx-auto my-2 pl-lg-1">
          <img src="/img/fms-3.png" class="img-fluid mx-auto rounded border p-1 bg-white" alt="">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data(){
    console.log(this.$route)
    return{
      mobile:window.innerWidth<991
    }
  }
}
</script>

<style>
.p-herramientas{
  padding-top: 5rem;
}
.rotate-45{
  transform: rotate(45deg);
}
.scroll-icon{
  position:absolute;
  top:95vh;
  width:100%;
  animation: scroll-icon 1.5s ease-in-out infinite ;
  transition: 0.5s all;
  filter: opacity(1);
  transform:translateY(-60%);
}

@keyframes scroll-icon {
	0% {
    transform: translateY(-60%);
    filter: opacity(0);
  }
	34% {
		transform:translateY(-150%);
    filter: opacity(1);
	}
	34% {
    transform: translateY(-60%);
    filter: opacity(0);
  }
	66% {
		transform:translateY(-150%);
    filter: opacity(1);
	}
	100% {
		transform:translateY(-150%);
    filter: opacity(0);
	}
}

.bg-home{
  background: #8E2DE2;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #4A00E0, #8E2DE2);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #4A00E0, #8E2DE2); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
.rta{
  color: white; /* Fallback: assume this color ON TOP of image */
  background: #8E2DE2;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #4A00E0, #8E2DE2);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #4A00E0, #8E2DE2); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.banner{
  height:99vh;
  background: #8E2DE2;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #4A00E0, #8E2DE2);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #4A00E0, #8E2DE2); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  -webkit-clip-path: polygon(0 0, 100% 0, 100% 90%, 50% 100%, 0 90%);
  clip-path: polygon(0 0, 100% 0, 100% 90%, 50% 100%, 0 90%);
}
</style>