import axios from 'axios';
<template>
  <div>
    <img src="https://concordeeducation.com/games/wp-content/uploads/sites/3/2022/08/Pokemon-Logo.png" alt="Pokemon Logo" />
    <h2>¿Quién es ese Pokémon?</h2>
    <p>Pokemons descubiertos: {{ discoveredCount }} </p>
    <div class="pokemon-list">
      <div v-for="pokemon in pokemons" :key="pokemon.id">
        <PokemonCard :pokemon="pokemon" @pokemon-guessed="counterUpdate" />
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
      guessedPokemons: [],
    };
  },
  async mounted() {
    const totalPokemons = 649;
    const limit = 20;
    const offset = Math.floor(Math.random() * (totalPokemons - limit));
    try {
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
  },
  computed: {
    discoveredCount() {
      return this.guessedPokemons.length;
    }
  },
  methods: {
    counterUpdate(pokemon) {
      this.guessedPokemons.push(pokemon.id);
    },
  },
};
</script>

<style scoped>
.pokemon-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
img{
  width: 200px;
  margin: 0 auto;
}
h2{
  margin: 0;
}
</style>
