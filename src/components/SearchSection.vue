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
                switchButton: false
            }
        },
        mounted(){
            console.log(this.switchButton);        
        },
        
        methods: {
            fetchData() {
                this.switchButton = false
                this.hoveredResult = {};
                axios.get(`https://pokeapi.co/api/v2/pokemon/${this.searchQuery}`).then((resp)=> {
                    this.result = resp.data;
                    console.log(this.hoveredResult);
                    
                })
            },
            catchPokemon() {
                this.switchButton = true
                console.log(this.switchButton);        
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
        <div>
            <button v-if="switchButton" class="fs-5" @click="catchPokemon">Remove</button>
            <button v-else class="fs-5" @click="catchPokemon">Catch</button>
        </div>
    </div>

    <div class="results-container d-flex flex-column justify-content-center align-items-center" v-if="hoveredPokemon && hoveredPokemon.name">
        <div class="img-cont">
            <img class="img-fluid" :src="hoveredPokemon.sprites.front_default" alt="">
        </div>
        <ul>
            <li>name - {{ hoveredPokemon.name }}</li>
            <li>type -  {{ hoveredPokemon.types[0].type.name }}</li>
            <li>height - {{ hoveredPokemon.height }}</li>
            <li>weight - {{ hoveredPokemon.weight }}</li>
            <hr>
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
            <hr>
            <li v-for="stats in result.stats">{{ stats.stat.name }} - {{ stats.base_stat }}</li>
        </ul>
    </div>

    <div class="results-container d-flex flex-column justify-content-center align-items-center" v-else>
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
            padding: 10px;
            font-size: 0.9rem;
            margin: 10px 0;
            height: 100%;
            width: 100%;
            background-color: green;
            border:  8px solid greenyellow;
            border-radius: 5px;
            color: greenyellow;
            li{
                text-transform: capitalize;
            }
        }
    }
</style>