<script>
import SearchSection from "./SearchSection.vue";
import CatchedSection from "./CatchedSection.vue"
import axios from "axios";

export default {
    components: {
        SearchSection,
        CatchedSection
    },
    data() {
        return {
            myPokemons: [],
            hoveredPokemon: {},
            showAlert: false,
        }
    },
    mounted() {
        const savedPokemons = localStorage.getItem('myPokemons');
        if (savedPokemons) {
            this.myPokemons = JSON.parse(savedPokemons);
        }
    },
    methods: {
        catchPokemons(pokemonName) {
            if (this.myPokemons.includes(pokemonName)){
                this.showAlert = true

                setTimeout(()=> {
                    this.showAlert = false
                }, 4000)
            } else {
                this.myPokemons.push(pokemonName)
                localStorage.setItem('myPokemons', JSON.stringify(this.myPokemons));
            }
        },
        showHoveredPokemon(pokemonHovered){
            if (pokemonHovered) {
                axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemonHovered}`).then((resp)=> {
                        this.hoveredPokemon = resp.data;
                })
            } else {
                this.hoveredPokemon = null
            }
        },

        deletePokemons(pokemonToDelete) {
            let index = this.myPokemons.findIndex(pokemon => pokemon === pokemonToDelete);

            if (index !== -1) {
                this.myPokemons.splice(index, 1);

                localStorage.setItem('myPokemons', JSON.stringify(this.myPokemons));
            }

        }
    }

}

</script>

<template>
    <!-- Alert pokemon already catched -->
    <div v-if="this.showAlert" class="alert alert-warning d-flex justify-content-center align-items-center mb-0 p-0">
        <div>
            <p class="fs-2 mb-0">You already got this pokemon!</p>
            <p class="fs-6 mb-0 text-center">Try catching another one.</p>
        </div>
    </div>

    <!-- sections -->
    <div id="pokedex-container" class="d-flex">
        <div class="ms-cont ms-cont-1">
            <SearchSection @pokemonName="catchPokemons" @pokemonToDelete="deletePokemons" :hoveredPokemon="hoveredPokemon" :myPokemons="myPokemons"/>
        </div>
        <div class="ms-cont ms-cont-2 d-flex align-items-center justify-content-center">
            <CatchedSection :pokemonList="myPokemons" @pokemonHovered="showHoveredPokemon"/>
        </div>
    </div>

</template>

<style scoped lang="scss">
    #pokedex-container{
        height: calc(90vh - 80px);
        width: 80%;
        max-width: 850px;
        margin: 2rem auto;
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