<template>
    <div>
        <div class="text-light my-4">
            <p class="my-2 font-weight-bold h5">Notas de versión</p>
            <div class="container rounded border border-light p-3" style="height:50vh; overflow: auto">
                <div>
                    <p class=" font-weight-bold my-0 h5">1.4</p>
                    <ul>
                        <li>Se añadió la función para ver el autómata que se está realizando</li>
                        <li>Se añadió al función para ver un autómata que se encuentra en el historial</li>
                    </ul>
                </div>
                <div>
                    <p class=" font-weight-bold my-0 h5">1.3</p>
                    <ul>
                        <li>Posibilidad de realizar autómatas de Turing</li>
                        <li>Se mejoró la intefaz y la visualización de la tabla</li>
                    </ul>
                </div>
                <div>
                    <p class=" font-weight-bold my-0 h5">1.2</p>
                    <ul>
                        <li>Posibilidad de realizar autómatas de pila</li>
                        <li>Se agregó el historial en el cual aparecen los autómatas con los se validó alguna palabra</li>
                    </ul>
                </div>
                <div>
                    <p class=" font-weight-bold my-0 h5">1.1</p>
                    <ul>
                        <li>Se añadió la posibilidad de eliminar estados y transiciones</li>
                        <li>Se agregaron las instrucciones</li>
                    </ul>
                </div>
                <div>
                    <p class=" font-weight-bold my-0 h5">1.0</p>
                    <ul>
                        <li>Reconocedor de palabras en autómatas finitos ingresados mediante la tabla de transición</li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="text-light my-4">
            <p class="my-2 font-weight-bold h5">Creador</p>
            <div class="container rounded border border-light px-3 py-2 align-middle">
                <div class="row mx-auto">
                    <div class="col-6 col-md-10 order-2 order-md-1 p-2 my-auto">
                        <p>Soy Matías Obezzi,<br class="d-md-none"/> tengo {{edad}} años.</p>
                        <p class="d-none d-lg-inline-block">Actualmente se encuentra en su versión {{version}} luego de 1 més de su lanzamiento.</p>
                        <p class="d-none d-lg-inline-block">
                            Esta app la realizé con el fin de brindarle una herramienta a quienes están estudiando o realizando autómatas.
                            La misma puede hacer desde la visualización hasta la evaluacón de si una palabra es reconocida por el mismo.
                        </p>
                    </div>
                    <div class="col-6 col-md order-1 my-md-2 my-2"><img src="https://matias-obezzi.github.io/media/foto-cv.png" class="rounded-circle border border-light bg-danger img-fluid" id="foto"></div>
                </div>
                <div class="row mx-auto px-2 my-0">
                    <p class="d-none d-lg-inline-block">Actualmente se encuentra en su versión {{version}} luego de 1 més de su lanzamiento.</p>
                    <p class="d-none d-lg-inline-block">
                        Esta app la realizé con el fin de brindarle una herramienta a quienes están estudiando o realizando autómatas.
                        La misma puede hacer desde la visualización hasta la evaluacón de si una palabra es reconocida por el mismo.
                    </p>
                    <p>
                        Próximamente voy a estar presentando la app como proyecto al CoNaIISI (Congreso Nacional de Ingeniería Informática), con
                        la ayuda de los profesores de la asignatura que me impulso a realizar la app.
                    </p>
                    <p>
                        Soy estudiante de Ingeniería en Sistemas de Información de la UTN FRBA, cursando 2do año y actualmente formo parte del CIIE 
                        (Centro de Investigación e Innovación en la Educación). Además utilizo gran parte de mi tiempo en aprender sobre tecnologías web
                        ya que soy autodidacta.
                    </p>
                    <p class="mb-0">
                        En fin, gracias por utilizar la app, espero que te haya sido de ayuda y podes visitar mi <a class="link" href="https://matias-obezzi.github.io" target="_blank">portfolio</a>.
                    </p>
                </div>
            </div>
        </div>
        <Instalar v-if="showInstall" :deferredPrompt="deferredPrompt" />
    </div>
</template>
<script>
import { version } from '@/../package.json';
import Instalar from '@/components/Instalar.vue'
export default {
    components:{Instalar},
    data(){
        return{
            edad: this.getEdad(),
            version,
            mobile:window.innerWidth<991,
            tamFoto:null
        }
    },
    props:['deferredPrompt', 'showInstall'],
    mounted(){
        this.tamFoto = document.querySelector("#foto").width+30;
    },
    methods:{
        getEdad(){
            var today = new Date();
            var birthDate = new Date("2000/06/22");
            var age = today.getFullYear() - birthDate.getFullYear();
            var m = today.getMonth() - birthDate.getMonth();
            if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
                age--;
            }
            return age;
        }
    }
}
</script>
<style scoped>
ul{
    list-style-type: disc;
}
</style>