<template>
  <div class="pokemon-card">
    <img
      :src="pokemon.sprites?.front_default"
      :alt="pokemon?.name"
      :style="{ filter: isGuessed ? 'none' : 'blur(5px)' }"
    />
    <h2 v-if="isGuessed">{{ pokemon.name }}</h2>
    <div v-else class="card-reveal">
      <input type="text" v-model="pokemonName" @keyup.enter="reveal" />
      <button @click="reveal">Descubrir</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "PokemonCard",
  data() {
    return {
      pokemonName: "",
      isGuessed: false,
    };
  },
  props: {
    pokemon: Object,
  },
  methods: {
    reveal() {
      this.isGuessed =
        this.pokemonName.toLowerCase() === this.pokemon.name.toLowerCase();
      if (this.isGuessed) {
        this.$emit("pokemon-guessed", this.pokemon);
      }
    },
  },
};
</script>

<style scoped>
.pokemon-card {
  border-radius: 5px;
  padding: 5px;
  margin: 10px;
  text-align: center;
  width: 160px;
  height: 200px;
}
.card-reveal {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
