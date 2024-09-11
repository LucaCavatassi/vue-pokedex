<script>

</script>

<template>
    <div id="pokedex-container" class="d-flex">
        <div class="ms-cont ms-cont-1">

        </div>
        <div class="ms-cont ms-cont-2">

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