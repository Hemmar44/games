<template>
    <div id="ship">
    </div>
</template>

<script>
    export default {
        name: "ShipComponent",
        data() {
            return {
                gameAreaData: {
                  width: 0,
                  height: 0
                },
                playerData: {
                    width: 0,
                    height: 0,
                    maxMoveRight: 0,
                    maxMoveLeft: 0,
                    maxMoveUp: 0,
                    maxMoveBottom: 0
                },
                gameAreaElement: null,
                playerElement: null
            }
        },
        mounted() {
            this.gameAreaElement = document.getElementById('game-area');
            this.playerElement = document.getElementById('ship');
            this.getGameAreaPosition();
            this.getPlayerPosition();
            this.countMovement();
            this.moveRight();
        },
        methods: {
            getGameAreaPosition() {
                const positionInfo = this.gameAreaElement.getBoundingClientRect();
                this.gameAreaData.width = positionInfo.right - positionInfo.left;
                this.gameAreaData.height = positionInfo.bottom - positionInfo.top;
            },
            getPlayerPosition() {
                const playerPositionInfo = this.playerElement.getBoundingClientRect();
                this.playerData.width = playerPositionInfo.right - playerPositionInfo.left;
                this.playerData.height = playerPositionInfo.bottom - playerPositionInfo.top;
            },
            countMovement() {
                let left = parseFloat(window.getComputedStyle(this.playerElement).getPropertyValue('left'));
                let top = parseFloat(window.getComputedStyle(this.playerElement).getPropertyValue('top'));
                this.playerData.maxMoveRight = this.gameAreaData.width - left - this.playerData.width;
                this.playerData.maxMoveLeft = left;
                this.playerData.maxMoveUp = top;
                this.playerData.maxMoveDown = this.gameAreaData.height - top - this.playerData.height;
            },
            moveRight() {
                let left = window.getComputedStyle(this.playerElement).getPropertyValue('left');
                this.playerElement.style.left = '200px';
            }
        }
    }
</script>

<style scoped>
    #ship {
        width: 20px;
        height: 20px;
        background-color: white;
        position: absolute;
        left: 50px;
        top: 100px;
    }
</style>