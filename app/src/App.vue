<template>
  <div>
    <AppHeader @pokemonName="getPokemon"/>
    <AppPokemonContainer :pokemonSearch="namePokemon"/>
    <AppFooter/>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import AppPokemonContainer from './components/pokemon/AppPokemonContainer.vue'
import AppFooter from './components/AppFooter.vue'

export default {
  name: 'App',
  components: {
    AppHeader,
    AppPokemonContainer,
    AppFooter
  },
  data(){
    return {
      namePokemon: '',
    }
  },
  methods: {
    getPokemon(name){
      this.namePokemon = name
    }
  }
}
</script>

<style>
  *{
    margin: 0;
    padding: 0;
    font-family: 'Quicksand', sans-serif;
    box-sizing: border-box;
  }
</style>
