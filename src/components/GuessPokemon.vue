import axios from 'axios';
<template>
  <div>
    <h1>Guess the Pokemon</h1>
    <div class="pokemon-list">
      <div v-for="(pokemon, index) in pokemons" :key="index">
        <PokemonCard :pokemon="pokemon" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PokemonCard from "./PokemonCard.vue";
export default {
  name: "GuessPokemon",
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemons: [],
    };
  },
  async mounted() {
    try {
      const totalPokemons = 1010;
      const limit = 20;
      const offset = Math.floor(Math.random() * (totalPokemons - limit));
      const { data } = await axios.get(
        `https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset}`
      );
      const pokemonResponses = await axios.all(
        data.results.map((pokemon) => axios.get(pokemon.url))
      );
      this.pokemons = pokemonResponses.map((response) => response.data);
    } catch (error) {
      console.error(error);
    }
    console.log(this.pokemons);
  },
};
</script>

<style scoped>
.pokemon-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
