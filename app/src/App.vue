<template>
  <div>
    <AppHeader/>
    <AppPokemonContainer/>
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
  }
}
</script>

<style>
  *{
    margin: 0;
    padding: 0;
    font-family: Arial, Helvetica, sans-serif;
    box-sizing: border-box;
  }
</style>
