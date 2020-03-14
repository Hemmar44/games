<template>
    <div id="game-area">
        <ship-component></ship-component>
    </div>
</template>

<script>
    export default {
        name: "GameComponent"
    }
</script>

<style scoped>
    #game-area {
        background-color: black;
        width: 80%;
        margin: 0 auto;
        height: 500px;
        position: relative;
    }
</style>