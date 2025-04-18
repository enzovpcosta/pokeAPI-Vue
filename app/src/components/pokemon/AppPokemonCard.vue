<template>
    <div class="card">
        <img :src="src" :alt="alt">
        <div class="card-info">
            <h3> {{ pokemonId }} </h3>
            <h3> {{ name }}</h3>
        </div>
    </div>
</template>

<script>
    export default{
        name: "AppPokemonCard",
        props: {
            src: String,
            alt: String,
            name: String,
            pokemonId: Number
        }
    }
</script>

<style scoped>
    .card{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.15);
    }

    .card-info{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 5px;
        border-top: 3px solid black;
        padding: 5px;
    }

</style>