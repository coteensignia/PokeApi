<script>
export default {
  name: 'PokemonCard',
  props: {
    pokemonName: {
      type: String
    },
    imageUrl: {
      type: String
    }
  },
  data() {
    return {
      userInput: '', // adivinación del usuario
      correct: false // variable para saber si ha sido encontrado
    }
  },
  methods: {
    /**
     * Método que se activa cuando queremos enviar información
     */
    sendName() {
      /**
       * Contrasta input usuario con el nombre pokemon
       */
      if (this.userInput.toLowerCase().trim() === this.pokemonName.toLowerCase()) {
        this.correct = true // variable "encontrado" ahora es true
        this.$emit('pokemon-discovered', this.pokemonName) // envia el nombre del pokemon al padre
      } else {
        alert('Siga participando')
      }
    }
  }
}
</script>

<template>
  <div class="card card-title text-center col-3 align-items-center pokemon-card">
    <img :class="!correct ? 'filter' : ''" :src="imageUrl" :alt="pokemonName" />
    <div v-if="correct">
      <p class="name">{{ pokemonName }}</p>
    </div>
    <form v-else @submit.prevent="sendName">
      <input v-model="userInput" class="input-field"/>
      <button class="submit-button">Descubrir</button>
      <p class="incorrect" v-show="!correct">Atina a tu Pokemon</p>
    </form>
  </div>
</template>

<style scoped>
.filter {
  filter: blur(5px) grayscale(100%);
}

.pokemon-card {
  border: 3px solid red; 
  background-color: white; 
  margin: 10px; 
  padding: 10px; 
  border-radius: 8px; 
  transition: filter 0.5s ease-in-out; 
}

.pokemon-card img {
  max-width: 100%; 
}

.input-field {
  margin-bottom: 10px; 
}

.submit-button {
  background-color: blue; 
  color: white; 
  padding: 8px 16px; 
  border: none; 
  border-radius: 4px; 
  font-size: 1rem; 
  cursor: pointer; 
}
</style>
