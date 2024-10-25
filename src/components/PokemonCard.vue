<script>
import axios from 'axios';

export default {
  props: ['pokemon'],
  data() {
    return {
      nombreIngresado: '',
      descubierto: false,
      pokemonImage: '',
    };
  },
  methods: {
    async obtenerImagen() {
      const response = await axios.get(this.pokemon.url);
      this.pokemonImage = response.data.sprites.front_default;
    },
    verificarNombre() {
      if (this.nombreIngresado.toLowerCase() === this.pokemon.name.toLowerCase()) {
        this.descubierto = true;
        this.$emit('descubierto');
      } else {
        alert('Nombre incorrecto, intenta de nuevo.');
      }
    },
  },
  mounted() {
    this.obtenerImagen();
  },
};
</script>
<template>
  <div class="pokemon-card">
    <img :src="pokemonImage" :style="{ filter: descubierto ? 'none' : 'blur(5px) grayscale(100%)' }" alt="pokemon" />
    <input type="text" class="input input-bordered input-success w-full max-w-xs" v-if="!descubierto" v-model="nombreIngresado" @keyup.enter="verificarNombre" />
    <button class="btn btn-active btn-secondary" v-if="!descubierto" @click="verificarNombre">Descubrir</button>
    <p  v-if="descubierto" :style="{ fontSize: '20px', marginLeft: '20px', color: '#008f4c', fontWeight: 'bold', textTransform: 'capitalize',filter: 'drop-shadow(2px 2px 4px rgba(0, 143, 76, 0.744))' }">{{ pokemon.name }}</p>
  </div>
</template>
<style scoped>
.btn{
  margin-top: 10px;
  text-align: center;
}
</style>