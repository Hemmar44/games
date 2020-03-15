<template>
    <div>
        {{gameData.points}}
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
                },
                gameData: {
                    points: 0
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
                Object.assign(this.playerData, player);
                this.performPlayerMove();
            },
            assignCollectiblesData(collectibles) {
                this.collectibles = collectibles;
            },
            getGameAreaPosition() {
                const positionInfo = this.gameAreaElement.getBoundingClientRect();
                this.gameAreaData.width = positionInfo.right - positionInfo.left;
                this.gameAreaData.height = positionInfo.bottom - positionInfo.top;
            },
            performPlayerMove() {
                if (this.collectibles.length > 0) {
                    this.collectibles.forEach((collectible) => {
                        if (this.touched(collectible.raw, collectible.touched)) {
                            collectible.touched = true;
                            this.gameData.points += collectible.points;
                            document.getElementById(collectible.id).style.display = "none";
                        }
                    });
                    if (this.isThisTheEnd(this.collectibles)) {
                        alert('koniec');
                    }
                }
            },
            isThisTheEnd(collectibles) {
                return collectibles.filter(collectible => !collectible.touched && collectible.proper).length === 0;
            },
            touched(positions, touched) {
                return this.horizontal(positions) && this.vertical(positions) && !touched;
            },
            horizontal(positions) {
                return this.between(positions.left, this.playerData.positionLeft, this.playerData.positionRight)
                    || this.between(positions.right, this.playerData.positionLeft, this.playerData.positionRight);
            },
            vertical(positions) {
                return this.between(positions.top, this.playerData.positionTop, this.playerData.positionBottom)
                    || this.between(positions.bottom, this.playerData.positionTop, this.playerData.positionBottom);
            },
            between(number, min, max) {
                return (number > min) && (number < max);
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