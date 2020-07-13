<template>
    <b-navbar toggleable="lg" type="dark" :class="{'bg-dark ':$route.name!='Home', 'bg-home fixed-top':$route.name=='Home'}">
        <b-navbar-brand @click="showNav = false" :to="{name:'Home'}" v-if="$route.name!='Home' || mobile"><img src="/favicon.png" height="40" class="invertir"></b-navbar-brand>

        <b-button @click="showNav = !showNav" type="button" variant="transparent" class="text-white d-lg-none d-inline-block ml-auto">
            <i class="fas fa-bars p-0" v-if="!showNav"></i>
            <i class="fas fa-times p-0" v-else></i>
        </b-button>

        <b-collapse class="p-0" id="nav-collapse" is-nav v-model="showNav">
            <b-navbar-nav :class="{'mx-auto':$route.name=='Home'}">
                <b-nav-item :to="{name:'Home'}" :class="{'active':$route.name=='Home'}">Inicio</b-nav-item>
                <b-nav-item :to="{name:'Reconocedor'}" :class="{'active':$route.name=='Reconocedor'}">Reconocedor</b-nav-item>
                <b-nav-item :to="{name:'About'}" :class="{'active':$route.name=='About'}">Sobre</b-nav-item>
            </b-navbar-nav>
        </b-collapse>
    </b-navbar>
</template>

<script>
export default {
    data(){
        return{
            showNav:false,
            opacity: 0,
            mobile: window.innerWidth<991
        }
    }
}
</script>

<style>
.invertir{
    filter:invert(1);
}
@media (min-width: 991px){
    .active{
        font-weight: bold;
        border-bottom-color: white;
        border-bottom-width: 2px;
        border-bottom-style: solid;
    }
}
</style>