<script>
    import axios from "axios";
    export default {
        emits: ["pokemonName"],
        props: {
            hoveredPokemon: {
                type: [Object, null],
                required: true
            }
        },
        data() {
            return {
                searchQuery: "",
                result: {},
                hoveredResult: {},
            }
        },
        mounted(){
            console.log(this.hoveredPokemon);        
        },
        methods: {
            fetchData() {
                axios.get(`https://pokeapi.co/api/v2/pokemon/${this.searchQuery}`).then((resp)=> {
                    this.result = resp.data;
                })
            },
            catchPokemon() {
                this.$emit("pokemonName", this.result.name)
            },
        }
    }
</script>

<template>
    <input type="text" v-model=searchQuery>
    <button @click="fetchData">Search</button>
    <button @click="catchPokemon">Catch</button>

    <ul v-if="hoveredPokemon.name">
        <img :src="hoveredPokemon.sprites.front_default" alt="">
        <li>{{ hoveredPokemon.name }}</li>
        <li>{{ hoveredPokemon.types[0].type.name }}</li>
        <li>{{ hoveredPokemon.height }}</li>
        <li>{{ hoveredPokemon.weight }}</li>

        <ul v-for="stats in hoveredPokemon.stats" :key="stats.stat.name">
            <li>{{ stats.stat.name }} - {{ stats.base_stat }}</li>
        </ul>
    </ul>

    <ul v-else-if="result.name">
        <img :src="result.sprites.front_default" alt="">
        <li>{{ result.name }}</li>
        <li>{{ result.types[0].type.name }}</li>
        <li>{{ result.height }}</li>
        <li>{{ result.weight }}</li>

        <ul v-for="stats in result.stats" :key="stats.stat.name">
            <li>{{ stats.stat.name }} - {{ stats.base_stat }}</li>
        </ul>
    </ul>
</template>

<style>
</style>