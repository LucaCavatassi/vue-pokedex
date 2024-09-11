<template>
    <div>
        <h3>Pokemon List</h3>
        <ul>
            <li v-for="(pokemonName, index) in pokemonList" :key="index">
                <p @mouseover="handleHover(pokemonName)" @mouseleave="handleLeave">{{ pokemonName }}</p> 
            </li>
        </ul>
    </div>
</template>

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

<style></style>