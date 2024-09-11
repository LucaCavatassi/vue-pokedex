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
    <div class="search-section d-flex justify-content-between align-items-center py-2">
    <div class="d-flex">
        <input class="me-1 w-50 fs-5" type="text" v-model="searchQuery" placeholder="Search...">
        <button @click="fetchData" class="fs-5"><i class="fa-solid fa-magnifying-glass"></i></button>
    </div>

    <button id="catch-button"  class="fs-5" @click="catchPokemon">Catch</button>
    </div>

    <div class="results-container d-flex flex-column justify-content-center align-items-center" v-if="hoveredPokemon.name">
        <div class="img-cont">
            <img class="img-fluid" :src="hoveredPokemon.sprites.front_default" alt="">
        </div>
        <ul>
            <img :src="hoveredPokemon.sprites.front_default" alt="">
            <li>{{ hoveredPokemon.name }}</li>
            <li>{{ hoveredPokemon.types[0].type.name }}</li>
            <li>{{ hoveredPokemon.height }}</li>
            <li>{{ hoveredPokemon.weight }}</li>
            <li v-for="stats in hoveredPokemon.stats">{{ stats.stat.name }} - {{ stats.base_stat }}</li>
        </ul>
    </div>

    <div class="results-container d-flex flex-column justify-content-center align-items-center" v-else-if="result.name">
        <div class="img-cont">
            <img class="img-fluid" :src="result.sprites.front_default" alt="">
        </div>
        <ul>
            <li>name - {{ result.name }}</li>
            <li>type -  {{ result.types[0].type.name }}</li>
            <li>height - {{ result.height }}</li>
            <li>weight - {{ result.weight }}</li>
            <hr class="m-0">
            <li v-for="stats in result.stats">{{ stats.stat.name }} - {{ stats.base_stat }}</li>
        </ul>
    </div>

    <div class="results-container d-flex flex-column justify-content-center align-items-center" v-else-if="!result.name">
        <div class="img-cont">
            <img class="img-fluid" src="/public/pokeball.png" alt="">
        </div>
        <ul>
            <li>No valid pokemon selected!</li>
        </ul>
    </div>
</template>

<style scoped lang="scss">
    .search-section {
        height: 50px;
        padding: 10px;
    }
    .results-container {
        height: calc(100% - 50px - 1rem);
        margin: 0.7rem 1rem;
        .img-cont{
            border: 10px solid gray;
            border-radius: 15px;
            width: 220px;
            height: 170px;
            background-color: white;
            img {
                border: 5px solid black;
                width: 100%;
                height: 100%;
                object-fit: contain;
            }
        }
        ul{
            list-style: none;
            border: 1px solid black;
            border-radius: 5px;
            padding: 10px;
            font-size: 0.9rem;
            margin: 10px 0;
            height: 100%;
            width: 100%;
            background-color: green;
            color: greenyellow;
            li{
                text-transform: capitalize;
            }
        }
    }
</style>