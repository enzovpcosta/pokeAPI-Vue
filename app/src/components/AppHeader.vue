<template>
    <header>
        <div class="logo">
            <h1>CostaDex</h1>
            <img :src="src" :alt="alt">
        </div>
        <div class="search">
            <input type="text" name="pokemon" id="pokemon" v-model="pokemon" placeholder="Digite o nome do pokémon" @input="$emit('pokemonName', pokemon)">
        </div>
    </header>
</template>

<script>
export default {
    name: "AppHeader",
    emits: ["pokemonName"],
    data(){
        return {
            src: "img/masterball.png",
            alt: "Master Ball icon",
            pokemon: null
        }
    }
}
</script>

<style scoped>
    header {
        padding: 20px 100px;
        background-color: #0D0D0D;
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 15vh;
    }

    .logo{
        display: flex;
        align-items: center;
        gap: 30px;
    }

    h1{
        color: #bb86fc   ;
    }

    img{
        width: 80px;
        height: 80px;
    }

    .search{
        width: 30%;
    }

    #pokemon{
        padding: 10px;
        width: 100%;
        border-radius: 10px;
    }

    #pokemon:focus{
        outline-color: #bb86fc;
        caret-color: #bb86fc;
    }
</style>