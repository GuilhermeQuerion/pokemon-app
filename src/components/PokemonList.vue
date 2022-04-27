<template>
  <h1>Lista de Pokemons</h1>
  <LoaderDefault v-if="isLoading"/>
  <div
    class="div-pokemon"
    v-for="(pokemon, index) in list"
    :key="index"
    @click="selectPokemon(getPokemonId(pokemon.url))"
  >
    <img :src="getPokemonImage(getPokemonId(pokemon.url))" />
    {{ pokemon.name }}
  </div>
</template>

<script>
import { onMounted, ref } from "vue";
import {
  getPokemonList,
  getPokemonImageUrl,
} from "../service/pokemon-service.js";
import { useRouter } from "vue-router";
import LoaderDefault from "./LoaderDefault.vue";

export default {
  components: {
    LoaderDefault,
  },
  setup() {
    const list = ref([]);
    const router = useRouter();
    const isLoading = ref(false);

    onMounted(() => {
      isLoading.value = true;
      getPokemonList().then((resp) => {
        list.value = resp.results;
      }).finally(() =>{
          isLoading.value = false;
      });
    });

    const getPokemonId = (url) =>
      url.replace("https://pokeapi.co/api/v2/pokemon/", "").replace("/", "");

    const getPokemonImage = (id) => getPokemonImageUrl(id);

    const selectPokemon = (id) =>
      router.push({ name: "pokemon", params: { id } });

    return { list, getPokemonImage, getPokemonId, selectPokemon, isLoading };
  },
};
</script>

<style scoped>
.div-pokemon {
  display: flex;
  align-items: center;
  border: 1px solid lightgrey;
  border-radius: 0.2rem;
  margin: 0.5rem;
  cursor: pointer;
}
</style>