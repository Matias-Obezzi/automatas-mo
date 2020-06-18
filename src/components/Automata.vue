<template>
    <div>
        {{automata}}
    </div>
</template>

<script>
export default {
    props:['table'],
    data(){
        return{
            automata:{}
        }
    },
    watch:{
        table: function (newVal, oldVal){
            console.log(oldVal, "\n", newVal);
            this.getTable(newVal);
        }
    },
    methods:{
        getTable(val){
            this.automata = val;
        }
    }
}
</script>