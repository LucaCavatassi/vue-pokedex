<template>
    <div>
        <h3>Pokemon List</h3>
        <ul>
            <li v-for="(name, index) in pokemonList" :key="index">{{ name }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        pokemonList: {
            type: Array,
            required: true
        }
    }
};
</script>

<style></style>