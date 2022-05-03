<template>
  <div class="pokemon-container">
    <img
      :src="imgSrc"
      class="hidden-pokemon"
      alt="pokemon">

    <img
      v-if="showPokemon"
      :src="imgSrc"
      class="fade-in"
      alt="pokemon">
  </div>
</template>

<script>

export default {
  props: {
    pokemonId: {
      type: Number,
      required: true,
      defoult: 1
    },

    showPokemon:{
      type: Boolean,
      required: true,
      defoult: false
    },
  },

  computed: {
    imgSrc(){
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${this.pokemonId}.svg`
    }
  }

}
</script>

<style scoped>
.pokemon-container {
  height: 300px;
  padding: 15px 0;
  margin-bottom: 30px;
}
img {
    height: 200px;
    user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    -webkit-user-drag: none;
    -webkit-user-select: none;
}
.hidden-pokemon {
    filter: brightness(0);
}
</style>