<template>
    <div id="game-area">
        <player-component @player="assignPlayerData"></player-component>
    </div>
</template>

<script>
    import PlayerComponent from "./PlayerComponent";
    export default {
        name: "GameComponent",
        data() {
            return {
                playerData: {}
            }
        },
        components: {PlayerComponent},
        methods: {
            assignPlayerData(value) {
                Object.assign(this.playerData, value);
            }
        }
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