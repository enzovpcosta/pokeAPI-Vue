<template>
    <div class="container">
        <h1 v-if="!pokemons">Nenhum pokémon foi encontrado!</h1>
        <div class="pokemon-container">
            <AppPokemonCard v-for="pokemon in pokemons" :key="pokemon.data.id" :src="pokemon.data.sprites.front_default" :alt="pokemon.data.species.name + ' image'" :name="pokemon.data.species.name" :pokemonId="pokemon.data.id"/>
        </div>
        <div class="btn-container">
            <button class="more-pokemons-btn">Buscar mais pokémons</button>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import AppPokemonCard from './AppPokemonCard.vue';

    export default{
        name: "AppPokemonContainer",
        components: {
            AppPokemonCard
        },
        data(){
            return {
                pokemons: null
            }
        },
        methods: {
            async getPokemons(){
                let endpoints = []

                for(let i = 1; i<=54; i++){
                    endpoints.push(`https://pokeapi.co/api/v2/pokemon/${i}`)
                }

                const req = await axios.all(endpoints.map((endpoint) => axios.get(endpoint)))

                this.pokemons = req
                
            }
        },
        mounted(){
            this.getPokemons()
        }
    }
</script>

<style scoped>

    .pokemon-container{
        min-height: 70vh;
        padding: 30px 100px;
        display: grid;
        grid-template-columns: repeat(6, 1fr);
        row-gap: 50px;
    }

    h1{
        padding-top: 30px;
        text-align: center;  
    }

    .btn-container{
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 30px 0;
    }

    .more-pokemons-btn {
        padding: 15px 25px;
        cursor: pointer;
        background-color: black;
        color: #bb86fc;
        font-weight: bold;
        border-radius: 15px;
        border: 1px solid #bb86fc;
        transition: 0.4s ease;
        box-shadow: 0 0 10px transparent;
    }

    .more-pokemons-btn:hover {
        transform: scale(1.05);
        background-color: #1a1a1a;
        color: #ffffff;
        box-shadow: 0 0 15px #bb86fc, 0 0 30px #bb86fc70;
    }

</style>