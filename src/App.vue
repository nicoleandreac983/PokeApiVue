<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';
import pokemonLogo from '@/assets/pokemon.png';

export default {
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemones: [],
      contadorDescubiertos: 0
    };
  },
  mounted() { 
    this.obtenerPokemones();
  },
  computed: {
    pokemonImage() {
      return require('@/assets/pokemon.png'); 
    }
  },
  methods: {
    async obtenerPokemones() {
      try {
        const data = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');
        this.pokemones = data.data.results;
      } catch (error) {
        console.error("error al obtener pokemones:", error);
      }
    },
    incrementarContador() {
      this.contadorDescubiertos += 1;
    },
  },
  computed: {
    pokemonLogo() {
      return pokemonLogo
    }
  }
}
</script>

<template>
   <div class="image-container flex justify-center items-center mt-4">
    <img :src="pokemonLogo" alt="logo" class=""/>
  </div>

  <div class="pokemon-container">
    <h2>Pokémon descubiertos: <span class="contador">{{ contadorDescubiertos }}</span></h2>
  </div>
    <div class="pokemon-grid">
      <PokemonCard v-for="(pokemon, index) in pokemones" :key="index" :pokemon="pokemon"
        @descubierto="incrementarContador" />
    </div>

</template>

<style>
h2 {
  text-align: center;
  margin-top: 50px;
}
.contador{
  font-weight: bold;
  color: goldenrod;
  margin-left: 10px;
  font-size: 34px;
  filter: drop-shadow(2px 2px 4px rgba(255, 255, 255, 0.744));
}
.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}
.pokemon-container {
  margin-bottom: 40px;
}

</style>
