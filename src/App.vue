<script>
  import PokedexContainer from "./components/PokedexContainer.vue"

  export default {
    components: {
      PokedexContainer
    }
  }

</script>

<template>
  <PokedexContainer/>
</template>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Electrolize&display=swap');

  html, body {
  font-family: 'Electrolize', sans-serif;
}
  body {
    background-image: url(/bg_pokedex.webp);
    background-position: center;
    background-size: contain;
  }
</style>