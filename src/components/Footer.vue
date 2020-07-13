<template>
    <div class="mt-auto text-center py-2" :class="{'bg-dark text-light':$route.name!='Home', 'bg-light text-dark':$route.name=='Home'}">
        <a class="link my-auto" :class="{'text-light':$route.name!='Home', 'text-dark':$route.name=='Home'}" href="https://www.github.com/Matias-Obezzi" target="_blank">Matias Obezzi</a> - 2020
    </div>
</template>

<script>
export default {
    data(){
        return{
            mobile: window.innerWidth<991
        }
    }
}
</script>

<style scoped>
</style>