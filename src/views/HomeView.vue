<template>
<!--<div class="w-full flex justify-center">
    <input
      placeholder="Enter Pokemon here"
      type="text"
      class="mt-10 p-2 border-blue-500 border-2"
      v-model="text"
    />
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center" >
    <div
      class="ml-4 text-2xl text-blue-400"
      v-for="(pokemon, idx) in filteredPokemon"
      :key="idx"
    >
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}
      </router-link>
    </div>
  </div>-->
  <div class="flex flex-col space-y-4 items-start p-8 ">
    <div >
    <div
      class="ml-4 text-2xl flex justify-items-center   text-blue-400"
      v-for="(pokemon, idx) in pokemons"
      :key="idx"
      > 
      <div  class="pr-8 flex ">Pokemon Name : </div>
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
       {{ pokemon.name }}
      </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { reactive, toRefs, computed } from "vue";
export default {
  name: "HomeView",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: "",
      filteredPokemon: computed(() => updatePokemon()),
    });

    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }

    fetch("https://pokeapi.co/api/v2/pokemon")
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
      });
    return { ...toRefs(state) };
  },
};
</script>

<style scoped>
</style>