<script>
    import axios from "axios";
    export default {
        data() {
            return {
                searchQuery: "",
                result: {},
            }
        },
        methods: {
            fetchData() {
                axios.get(`https://pokeapi.co/api/v2/pokemon/${this.searchQuery}`).then((resp)=> {
                    this.result = resp.data;
                    this.$emit("pokemonName", this.result.name)
                })
            }
        }
    }
</script>

<template>
    <input type="text" v-model=searchQuery>
    <button @click="fetchData">Search</button>

    <ul v-if="this.result.name">
        <img :src="this.result.sprites.front_default" alt="">
        <li>{{ this.result.name }}</li>
        <li>{{ this.result.types[0].type.name }}</li>
        <li>{{ this.result.height }}</li>
        <li>{{ this.result.weight }}</li>

        <ul v-for="stats in this.result.stats">
            <li>{{ stats.stat.name }} - {{ stats.base_stat }}</li>
        </ul>



    </ul>
</template>

<style>
</style>