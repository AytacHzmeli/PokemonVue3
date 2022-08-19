<template>
  <div class="flex-col w-auto flex items-center">
    <div
      v-if="pokemon"
      class="bg-red-700 mt-2 flex flex-col items-center mt-10"
    >
      <h3 class="text-5xl text-blue-600 uppercase">{{ pokemon.name }}</h3>
      <div class="flex ">
        <img class="w-48" :src="pokemon.sprites.front_shiny" alt="" />
      </div>
      <ul class="type">
        <h2>Type:</h2>
        <li
          v-for="(type, key) in pokemon.types"
          :key="key"
          :class="type.type.name"
        >
          <span>{{ type.type.name }}</span>
        </li>
      </ul>
      <ul class="stats">
        <h2>Stats:</h2>
        <li v-for="(stat, key) in pokemon.stats" :key="key">
          <span>{{ stat.stat.name }} -> {{ stat.base_stat }}</span>
        </li>
      </ul>
    </div>
    <div class="flex flex-row justify-evenly mt-1 text-3xl bg-green-500">
      <button v-on:click="togglefav">
        {{ isFav ? "Favorilerden Kaldır" : "Favorilere Ekle" }}
      </button>
      <!--<button v-on:click="showfav">favorılere gözat</button>-->
    </div>
    <div class="flex flex-column mt-1 ">
     <main >
     <div>
		<button @click="() => TogglePopup('buttonTrigger')" class="text-3xl bg-green-500">Show Favorite List</button>
    </div>
		<Popup 
			v-if="popupTriggers.buttonTrigger" 
			:TogglePopup="() => TogglePopup('buttonTrigger')">
      <button v-on:click="showfav" class="bg-green-100">
      {{favdata}}
      </button> 
      <div  class="bg-gray-200">{{showfav}}</div>
      </Popup>
	</main>
  </div>
  </div>
  
</template>
<script>
import { useRoute } from "vue-router";
import { ref, computed } from "vue";
import MyPopup from '@/components/MyPopup.vue';

export default {
  setup() {
    
    const route = useRoute();
    const aytac=("sadasd");
    const pokemon = ref(null);
    const favorite = ref(JSON.parse(localStorage.getItem("favorite")) ?? []);
    const isFav = computed(() => favorite.value.includes(pokemon.value?.name));
    const showfav=ref(JSON.parse(localStorage.getItem("favorite")));
    const popupTriggers = ref({
			buttonTrigger: false,
		});

		const TogglePopup = (trigger) => {
			popupTriggers.value[trigger] = !popupTriggers.value[trigger]
		}
    
    function togglefav() {
      if (!isFav.value) {
        favorite.value.push(pokemon.value.name);
      } else {
        const index = favorite.value.indexOf(pokemon.value.name);
        if (index !== -1) {
          favorite.value.splice(index, 1);
        }
      }

      localStorage.setItem("favorite", JSON.stringify(favorite.value));
    }

    /*function showfav() {
      //alert(JSON.parse(localStorage.getItem("favorite")));
      //favorite.value=(JSON.parse(localStorage.getItem("favorite")));
      var favdata = localStorage.getItem("favorite");
      console.log("Gelen veri : " + favdata);
      
    }*/
    

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        pokemon.value = data;
      });

    return {
      pokemon,
      togglefav,
      showfav,
      isFav,
      MyPopup,
      aytac,
			popupTriggers,
			TogglePopup,
    };
  },
};
</script>

<style scoped>
.type {
  display: flex;
  background-color: rgb(228, 149, 212);
  width: 100%;
  justify-content: space-evenly;
}
</style>