<template>
  <div class="w-full flex justify-center">
    <input
      type="text"
      placeholder="Search"
      class="mt-10 p-2 border-blue-500 border-2"
      v-model="text"
    />
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div class="ml-4 text-2x text-blue-600" v-for="(item,id) in filteredPokemons" :key="id">
    <router-link :to="`/about/${urlId[item.name]}`">
    {{ item.name }}</router-link>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, computed } from "vue";

export default {
  name: 'Home',
  setup() {

    const state = reactive({
      pokemons: [],
      urlId: {},
      text: '',
      filteredPokemons: computed(() => updatePokemon())
    })

    function updatePokemon() {

      if (!state.text) {
        return []
      }
      return state.pokemons.filter((pokemon) => 
         pokemon.name.includes(state.text)
      )
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then(res => res.json())
      .then(data => {
        state.pokemons = data.results,
          state.urlId = data.results.reduce((acc, cur, idx) =>
            acc = { ...acc, [cur.name]: idx + 1 }, {})
      })

    return { ...toRefs(state) }
  }
}
</script>
