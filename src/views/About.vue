<template>
  <div class="about">
    <div class="flex justify-center"><button class="w-32 py-2 px-2 mb-3 rounded-full  bg-purple-800 text-white" @click="goBack()">Back</button></div>
    <div v-if="pokemon" class="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center">
    <h3 class="text-2xl text-green-900 uppercase">{{pokemon.name}}</h3>
    <div class="flex justify-center">
      <img class="w-48" :src="pokemon.sprites.front_shiny" :alt="pokemon.name">
      <img class="w-48" :src="pokemon.sprites.back_shiny" :alt="pokemon.name">
    </div>
    <h3 class="text-yellow-400">
      Types
    </h3>
    <div v-for="(type,id) in pokemon.types" :key="id">
    <h5 class="text-blue-900">
      {{type.type.name}}
    </h5>
    
    </div>
    </div>
  </div>
</template>


<script>

import { useRoute} from "vue-router";
import { reactive,toRefs} from "vue";

export default {


 setup() {
const route = useRoute();

const state = reactive({
  pokemon: null,

});

 fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
 .then((res) => res.json())
 .then((data) => {
   state.pokemon = data;
 })

 return {...toRefs(state)}

 },

 methods:{
    goBack(){
      this.$router.go(-1)
    }
 }
 
}

</script>