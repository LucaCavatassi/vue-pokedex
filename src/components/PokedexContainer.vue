<script>
import SearchSection from "./SearchSection.vue";
import CatchedSection from "./CatchedSection.vue"

export default {
    components: {
        SearchSection,
        CatchedSection
    },
    data() {
        return {
            myPokemons: []
        }
    },
    methods: {
        catchPokemons(pokemonName) {
            if (this.myPokemons.includes(pokemonName)){
                console.log("Pokemon already there!");
            } else {
                this.myPokemons.push(pokemonName)
            }
        }
    }

}

</script>

<template>
    <div id="pokedex-container" class="d-flex">
        <div class="ms-cont ms-cont-1">
            <SearchSection @pokemonName="catchPokemons"/>
        </div>
        <div class="ms-cont ms-cont-2">
            <CatchedSection :pokemonList="myPokemons"/>
        </div>
    </div>

</template>

<style scoped lang="scss">
    #pokedex-container{
        height: 70vh;
        width: 80%;
        margin: 2.5rem auto;
        background-color: rgb(253, 89, 89);
        border: 8px solid  #ff3434;
        border-radius: 15px;
        .ms-cont {
            height: 100%;
            width: calc(100% / 2);
        }
        .ms-cont-1{
            border-right: 4px solid  #ff3434;
        }
        .ms-cont-2{
            border-left: 4px solid  #ff3434;
        }
    }
</style>