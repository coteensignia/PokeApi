<script>
import axios from 'axios'
import PokemonCard from '@/components/PokemonCard.vue'

export default {
  name: 'PokemonList',
  components: { PokemonCard },
  data() {
    return {
      pokemonArray: [] 
    }
  },
  async created() {
    try {
      const pokemonPromises = []
      for (let i = 1; i <= 20; i++) {
        const pokemonURL = `https://pokeapi.co/api/v2/pokemon/${i}`
        pokemonPromises.push(axios.get(pokemonURL))
      }
      const results = await Promise.all(pokemonPromises)
      this.pokemonArray = results.map(result => ({ ...result.data, isDiscovered: false }))
      
    } catch (error) {
      console.error('No pudimos capturar los Pokémon, intentalo más tarde 🥺')
    }
  },
  computed: {
    discoveredCount() {
      return this.pokemonArray.reduce((total, pokemon) => total + (pokemon.isDiscovered ? 1 : 0), 0)
    }
  },
  methods: {
    markAsDiscovered(pokemonName) {
      const targetPokemon = this.pokemonArray.find(
        (pokemon) => pokemon.name.toLowerCase() === pokemonName.toLowerCase()
      )
      if (targetPokemon) {
        targetPokemon.isDiscovered = true
      }
    }
  }
}
</script>

<template>
  <div class="container">
    <div class="mt-3 d-flex flex-column align-items-center">
      <img src="/public/International_Pokémon_logo.svg.png" alt="logo_Pokemon" />
      <h1>¿Quién es este Pokémon?</h1>
      <p class="discovered-count">Pokémon descubiertos: {{ discoveredCount }}</p>
    </div>
    <div class="row mt-3">
      <PokemonCard
        v-for="(pokemon, index) in pokemonArray"
        :key="index"
        :pokemonName="pokemon.name"
        :imageUrl="pokemon.sprites.front_default"
        @pokemon-discovered="markAsDiscovered" 
        class="pokemon-card" 
      />
    </div>
  </div>
</template>

<style scoped>
.container {
  background-color: yellow; 
  display: flex;
  flex-direction: column;
  align-items: center;
}


.row {
  display: flex;
  flex-wrap: wrap;
  justify-content: center; 
}


.discovered-count {
  font-size: 1.5rem; 
  font-weight: bold; 
  color: blue;
}
</style>
