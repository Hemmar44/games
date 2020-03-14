<template>
    <div id="game-area">
        <player-component
            :game-area-data="gameAreaData"
            @player="assignPlayerData"
        ></player-component>
        <collectible-component
            :game-area-data="gameAreaData"
            @collectibles="assignCollectiblesData"
        ></collectible-component>
    </div>
</template>

<script>
    import PlayerComponent from "./PlayerComponent";
    export default {
        name: "GameComponent",
        data() {
            return {
                playerData: {},
                collectibles: [],
                gameAreaData: {
                    width: 0,
                    height: 0
                }
            }
        },
        mounted() {
            this.gameAreaElement = document.getElementById('game-area');
            this.getGameAreaPosition();
        },
        components: {PlayerComponent},
        methods: {
            assignPlayerData(player) {
                console.log('oooooo');
                Object.assign(this.playerData, player);
            },
            assignCollectiblesData(collectibles) {
                console.log('dddaaa');
                this.collectibles = collectibles;
            },
            getGameAreaPosition() {
                const positionInfo = this.gameAreaElement.getBoundingClientRect();
                this.gameAreaData.width = positionInfo.right - positionInfo.left;
                this.gameAreaData.height = positionInfo.bottom - positionInfo.top;
            },
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