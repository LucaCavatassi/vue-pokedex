<script>
export default {
    emits: ["pokemonHovered"],
    props: {
        pokemonList: {
            type: Array,
            required: true
        }
    },
    methods: {
        handleHover(pokemonNameHovered){
            this.$emit("pokemonHovered", pokemonNameHovered);
        },
        handleLeave() {
            this.$emit("pokemonHovered", null);
        }
    }
};
</script>

<template>
            <div class="catched-pokemons p-3">
                <h3>My Pokemons</h3>
                <ul class="p-0">
                    <li v-for="(pokemonName, index) in pokemonList" :key="index">
                        <p @mouseover="handleHover(pokemonName)" @mouseleave="handleLeave">{{ pokemonName }}</p> 
                    </li>
                </ul>
            </div>
</template>

<style scoped lang="scss">
    .catched-pokemons{
        background-color: white;
        height: calc(100% - 50px - 1rem);
        width: 80%;
        border: 8px solid black;
        border-radius: 15px;
        ul{
            list-style-type: none;
            li {
                text-transform: capitalize;
                p:hover{
                    color: rgb(97, 96, 96);
                }
            }
        }
    }
</style>