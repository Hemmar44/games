<template>
    <div id="ship">
    </div>
</template>

<script>
    export default {
        name: "ShipComponent",
        data() {
            return {
                maxMoveRight: 0,
                maxMoveLeft: 0,
                maxMoveUp: 0,
                maxMoveBottom: 0
            }
        },
        mounted() {
            const positionInfo = this.gameArea.getBoundingClientRect();
            const width = positionInfo.right - positionInfo.left;
            const height = positionInfo.bottom - positionInfo.top;
            console.log({'area-width': width, 'area-height': height});

            let right = parseFloat(window.getComputedStyle(this.vessel).getPropertyValue('right'));
            let left = parseFloat(window.getComputedStyle(this.vessel).getPropertyValue('left'));
            let top = parseFloat(window.getComputedStyle(this.vessel).getPropertyValue('top'));
            let bottom = parseFloat(window.getComputedStyle(this.vessel).getPropertyValue('bottom'));

            console.log(right, left, top, bottom);

            const vesselPositionInfo = this.vessel.getBoundingClientRect();
            const vesselWidth = vesselPositionInfo.right - vesselPositionInfo.left;
            const vesselHeight = vesselPositionInfo.bottom - vesselPositionInfo.top;
            console.log(vesselWidth, vesselHeight);

            this.maxMoveRight = width - left - vesselWidth;
            this.maxMoveLeft = left;
            this.maxMoveUp = top;
            this.maxMoveDown = height - top - vesselHeight;


            console.log({
                'right': maxMoveRight,
                'left': maxMoveLeft,
                'up': maxMoveUp,
                'down': maxMoveDown
            })
        },
        computed: {
            vessel: () => {
                return document.getElementById('ship');
            },
            gameArea: () => {
                return document.getElementById('game-area');
            }
        },
        methods: {
            moveRight: () => {

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
        left: 30px;
        top: 100px;
    }
</style>