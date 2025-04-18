<template>
    <footer>
        <p>Made By <a :href="linkedin" target="_blank">Enzo Costa</a>! &copy; 2025</p>
    </footer>
</template>

<script>
    export default{
        name: "AppFooter",
        data(){
            return {
                linkedin: "https://www.linkedin.com/in/enzovpcosta/"
            }
        }
    }
</script>

<style>
    footer{
        display: flex;
        align-items: center;
        justify-content: center;
        height: 15vh;
        background-color: #0D0D0D;
        color: white;
    }

    a{
        text-decoration: none;
        color: #bb86fc;
    }
</style>