<script>
    import axios from "axios";
    export default {
        emits: ["pokemonName", "pokemonToDelete", "isCatched"],

        props: {
            hoveredPokemon: {
                type: [Object, null],
                required: true
            },
            myPokemons: {
                type: [Array, null],
                required: true
            }
        },

        data() {
            return {
                searchQuery: "",
                result: {},
                hoveredResult: {},
                inArray: false,
                showTopImage: true,
            }
        },

        methods: {
            fetchData() {
                this.hoveredResult = {};
                axios.get(`https://pokeapi.co/api/v2/pokemon/${this.searchQuery}`).then((resp)=> {
                    this.result = resp.data;
                    if(this.myPokemons.includes(this.result.name)) {
                        this.inArray = true;
                    }else {
                        this.inArray = false;
                    }
                })                
            },

            handlePokemon() {     
                if (!this.inArray) {
                    this.$emit("pokemonName", this.result.name)
                    this.inArray = true;
                } else if (this.inArray) {
                    this.$emit("pokemonToDelete", this.result.name)
                    this.inArray = false;   
                }
            },

        },   
        computed: {
            topImageClass() {
            return this.showTopImage ? 'd-block' : 'd-none';
            },
            bottomImageClass() {
            return this.showTopImage ? 'd-none' : 'd-block';
            },
        },
        mounted() {
            setInterval(() => {
            this.showTopImage = !this.showTopImage;
            }, 1000); // 2 seconds interval
        },
    }
</script>

<template>
    <div class="search-section d-flex justify-content-between align-items-center py-2">
        <div class="d-flex">
            <input class="me-1 w-50 fs-5" type="text" v-model="searchQuery" placeholder="Search...">
            <button @click="fetchData" class="fs-5"><i class="fa-solid fa-magnifying-glass"></i></button>
        </div>
        <div>
            <button v-if="inArray" class="fs-5" @click="handlePokemon">Remove</button>
            <button v-else class="fs-5" @click="handlePokemon">Catch</button>
        </div>
    </div>

    <div class="results-container d-flex flex-column justify-content-center align-items-center" v-if="hoveredPokemon && hoveredPokemon.name">
        <section class="img-cont">
            <img class="img-fluid" :class="topImageClass" :src="hoveredPokemon.sprites.front_default" :alt="hoveredPokemon.name">
            <img class="img-fluid" :class="bottomImageClass" :src="hoveredPokemon.sprites.back_default" :alt="hoveredPokemon.name">
        </section>
        <ul>
            <li>name - {{ hoveredPokemon.name }}</li>
            <li>type -  {{ hoveredPokemon.types[0].type.name }}</li>
            <li>height - {{ hoveredPokemon.height }}</li>
            <li>weight - {{ hoveredPokemon.weight }}</li>
            <hr>
            <li v-for="stat in hoveredPokemon.stats" :key="stat.stat.name" class="stat-item">
                {{ stat.stat.name }}: 
                <div class="progress-bar">
                    <span :style="{ width: (stat.base_stat / 255) * 100 + '%' }"></span>
                </div>
            </li>
        </ul>
    </div>

    <div class="results-container d-flex flex-column justify-content-center align-items-center" v-else-if="result.name">
        <section class="img-cont">
            <img class="img-fluid" :class="topImageClass" :src="result.sprites.front_default" :alt="result.name">
            <img class="img-fluid" :class="bottomImageClass" :src="result.sprites.back_default" :alt="result.name">
        </section>
        <ul>
            <li>name - {{ result.name }}</li>
            <li>type -  {{ result.types[0].type.name }}</li>
            <li>height - {{ result.height }}</li>
            <li>weight - {{ result.weight }}</li>
            <hr>
            <li v-for="stat in result.stats" :key="stat.stat.name" class="stat-item">
                {{ stat.stat.name }}: 
                <div class="progress-bar">
                    <span :style="{ width: (stat.base_stat / 150) * 100 + '%' }"></span>
                </div>
            </li>
        </ul>
    </div>

    <div class="results-container d-flex flex-column justify-content-center align-items-center" v-else>
        <div class="img-cont">
            <img class="img-fluid" src="/pokeball.png" alt="">
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
    .stat-item {
        display: flex;
        align-items: center;
        margin: 5px 0;

        .progress-bar {
            margin-left: 10px;
            background-color: rgb(18, 187, 18);
            border-radius: 5px;
            overflow: hidden;
            flex-grow: 1;
            height: 10px;

            span {
                display: block;
                height: 100%;
                background-color: greenyellow; // Gray background for the progress bar
                border-radius: 5px;
                line-height: 20px; // Adjust based on height
            }
        }
    }
</style>