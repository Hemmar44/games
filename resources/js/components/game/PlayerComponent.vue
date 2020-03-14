<template>
    <div>
        <div id="player">
        </div>
        <Keypress :key-code="39" event="keydown" @pressed="moveRight" />
        <Keypress :key-code="37" event="keydown" @pressed="moveLeft" />
        <Keypress :key-code="38" event="keydown" @pressed="moveUp" />
        <Keypress :key-code="40" event="keydown" @pressed="moveDown" />
    </div>
</template>

<script>
    export default {
        name: "PlayerComponent",
        data() {
            return {
                playerData: {
                    width: 0,
                    height: 0,
                    borderRight: 0,
                    positionLeft: 0,
                    positionTop: 0,
                    borderBottom: 0
                },
                step: 20,
                playerElement: null,
            }
        },
        components:{
            Keypress: () => import('vue-keypress')
        },
        props: {
            gameAreaData: Object
        },
        mounted() {
            this.playerElement = document.getElementById('player');
        },
        watch: {
            gameAreaData: {
                handler() {
                    this.getPlayerPosition();
                    this.countMovement();
                    this.emitPlayerData(800);
                },
                deep: true
            }
        },
        methods: {
            getPlayerPosition() {
                const playerPositionInfo = this.playerElement.getBoundingClientRect();
                this.playerData.width = playerPositionInfo.right - playerPositionInfo.left;
                this.playerData.height = playerPositionInfo.bottom - playerPositionInfo.top;
            },
            countMovement() {
                let left = parseFloat(window.getComputedStyle(this.playerElement).getPropertyValue('left'));
                let top = parseFloat(window.getComputedStyle(this.playerElement).getPropertyValue('top'));
                this.playerData.borderRight = this.gameAreaData.width - this.playerData.width;
                this.playerData.positionLeft = left;
                this.playerData.positionTop = top;
                this.playerData.borderBottom = this.gameAreaData.height - this.playerData.height;
            },
            moveRight() {
                this.playerData.positionLeft += this.step;
                if (this.playerData.positionLeft >= this.playerData.borderRight) {
                    this.playerData.positionLeft = this.playerData.borderRight;
                }
                this.playerElement.style.left = this.playerData.positionLeft + 'px';
                this.emitPlayerData();
            },
            moveLeft() {
                this.playerData.positionLeft -= this.step;
                if (this.playerData.positionLeft <= 0) {
                    this.playerData.positionLeft = 0;
                }
                this.playerElement.style.left = this.playerData.positionLeft + 'px';
                this.emitPlayerData();
            },
            moveUp() {
                this.playerData.positionTop -= this.step;
                if (this.playerData.positionTop <= 0) {
                    this.playerData.positionTop = 0;
                }
                this.playerElement.style.top = this.playerData.positionTop + 'px';
                this.emitPlayerData();
            },
            moveDown() {
                this.playerData.positionTop += this.step;
                if (this.playerData.positionTop >= this.playerData.borderBottom) {
                    this.playerData.positionTop = this.playerData.borderBottom;
                }
                this.playerElement.style.top = this.playerData.positionTop + 'px';
                this.emitPlayerData();
            },
            emitPlayerData(time = 0) {
                setTimeout(() => {
                    this.$emit('player', this.playerData);
                    }, time);

            }
        }
    }
</script>

<style scoped>
    #player {
        width: 20px;
        height: 20px;
        background-color: white;
        position: absolute;
        left: 50px;
        top: 100px;
    }
</style>