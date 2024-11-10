<template>
    <div class="col-md-3 mb-4">
      <div class="card text-center">
        <img 
          :src="pokemon.image" 
          class="card-img-top" 
          :style="{ filter: isDiscovered ? 'none' : 'blur(5px) grayscale(100%)' }" 
          alt="Pokemon"
        />
        <div class="card-body">

          <h5 v-if="isDiscovered">{{ pokemon.name }}</h5>

          <div v-else>
            <input 
              type="text" 
              v-model="guess" 
              class="form-control mb-2" 
              @keyup.enter="checkGuess" 
              placeholder="¿Quién es este Pokémon?"
            />
            <button class="btn btn-primary" @click="checkGuess">Descubrir</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['pokemon'],
    data() {
      return {
        guess: '',
        isDiscovered: false,
      };
    },
    methods: {

      checkGuess() {
        if (this.guess.toLowerCase() === this.pokemon.name.toLowerCase()) {
          this.isDiscovered = true;
          this.$emit('discovered');
        } else {
          alert('¡Nombre incorrecto! Inténtalo de nuevo.');
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .card {
    border-radius: 10px;
  }
  .card-img-top {
    height: 150px;
    object-fit: cover;
  }
  </style>
  