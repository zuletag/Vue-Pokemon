<template>
  <div class="container text-center">
    <h1>Adivina el Pokémon</h1>
    <p>Pokémon descubiertos: {{ discoveredCount }}</p>
    <div class="row">
      <PokemonCard 
        v-for="pokemon in pokemons" 
        :key="pokemon.id" 
        :pokemon="pokemon" 
        @discovered="incrementCount"
      />
    </div>
  </div>
</template>

<script>
import PokemonCard from './components/PokemonCard.vue';
import axios from 'axios';

export default {
  components: { PokemonCard },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0,
    };
  },
  methods: {

    incrementCount() {
      this.discoveredCount++;
    },

    async loadPokemons() {
      try {

        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');

        const pokemonPromises = response.data.results.map(async (p) => {
          const details = await axios.get(p.url);
          return {
            id: details.data.id,
            name: details.data.name,
            image: details.data.sprites.front_default,
          };
        });
        this.pokemons = await Promise.all(pokemonPromises);
      } catch (error) {
        console.error('Error al cargar Pokémon:', error);
      }
    },
  },

  mounted() {
    this.loadPokemons();
  },
};
</script>

<style>
.container {
  padding-top: 2rem;
}
</style>
