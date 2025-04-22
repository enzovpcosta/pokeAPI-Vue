<template>
    <div class="container">
        <h1 v-if="isPokemonEmpty">Nenhum pokémon foi encontrado!</h1>
        <div class="pokemon-container">
            <AppPokemonCard v-for="pokemon in pokemons" :key="pokemon.data.id" :src="pokemon.data.sprites.front_default" :alt="pokemon.data.name + ' image'" :name="pokemon.data.name" :pokemonId="pokemon.data.id"/>
        </div>
        <h4 v-if="isAllPokemons">Todos os pokemons foram encontrados!</h4>
        <div v-if="seeMore && !isAllPokemons" class="btn-container">
            <button class="more-pokemons-btn" @click="getMorePokemons" :disabled="isLoadingMore">Buscar mais pokémons</button>
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
        props: {
            pokemonSearch: String
        },
        data(){
            return {
                pokemons: [],
                seeMore: true,
                currentPage: 1,
                perPage: 54,
                endpoints: [],
                abortController: null,
                isPokemonEmpty: false,
                isLoadingMore: false,
                isAllPokemons: false
            }
        },
        watch: {
            pokemonSearch(newVal, oldVal) {
                const newV = newVal.trim()
                const oldV = oldVal.trim()

                if (newV !== '' && newV !== oldV) {
                    this.getPokemon(newV)
                    return
                }

                if (newV === '' && newV !== oldV) {
                    this.pokemons = []
                    this.getPokemons()
                }
            }
        },
        methods: {
            async getPokemon(newVal){

                if(this.abortController){
                    this.abortController.abort()
                }

                this.abortController = new AbortController()

                try {
                    const req = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=10000', {
                        signal: this.abortController.signal
                    })
                    
                    const reqPokemons = req.data.results.filter(pokemon => pokemon.name.includes(newVal.toLowerCase()))

                    this.isPokemonEmpty = reqPokemons.length === 0;

                    if(!this.isPokemonEmpty){
                        
                        this.endpoints = []
                        
                        for(let i = 0; i<reqPokemons.length; i++){
                            this.endpoints.push(`https://pokeapi.co/api/v2/pokemon/${reqPokemons[i].name}`)
                        }
                        
                        const response = await axios.all(
                            this.endpoints.map((endpoint) => 
                            axios.get(endpoint, { signal: this.abortController.signal}) 
                            )
                        )
                    
                        this.pokemons = response
                    } else {
                        this.pokemons = null
                    }

                    this.currentPage = 1
                    this.seeMore = false
                    this.endpoints = []

                } catch (error) {
                    console.log('Algo deu errado!');
                }
            },
            async getPokemons(){
                if(this.abortController){
                    this.abortController.abort()
                }

                this.abortController = new AbortController()

                try{
                    
                    const start = (this.currentPage - 1) * this.perPage + 1;
                    const end = this.currentPage * this.perPage >= 1025 ? 1025 : this.currentPage * this.perPage;

                    if(!this.isAllPokemons){
                        
                        const endpoints = [];
                        
                        for (let i = start; i <= end; i++) {
                            endpoints.push(`https://pokeapi.co/api/v2/pokemon/${i}`);
                        }
                        
                        const req = await axios.all(endpoints.map(endpoint => axios.get(endpoint, {signal: this.abortController.signal})));
                        
                        req.map(response => this.pokemons.push(response))
                        this.seeMore = true;
                    }
                    
                    this.isAllPokemons = end === 1025;
                    
                } catch (error) {
                    console.log(error);
                    
                }

            },
            async getMorePokemons(){
                this.isLoadingMore = true

                this.currentPage++

                await this.getPokemons()

                this.isLoadingMore = false
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
        gap: 50px;
    }

    h1{
        padding-top: 30px;
        text-align: center;  
    }

    h4{
        color: #bb86fc;
        text-align: center;
        padding-bottom: 30px;
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

    @media (max-width: 1100px){
        .pokemon-container{
            grid-template-columns: repeat(4, 1fr);
        }
    }
    
    @media (max-width: 850px){
        .pokemon-container{
            grid-template-columns: repeat(3, 1fr);
            column-gap: 20px;
            row-gap: 50px;
        }
    }

    @media (max-width: 650px){
        .pokemon-container{
            padding: 30px 50px;
            grid-template-columns: repeat(2, 1fr);
            column-gap: 20px;
            row-gap: 50px;
        }
    }
</style>