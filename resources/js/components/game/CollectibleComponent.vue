<template>
    <div>
        <div v-for="element in collectibles">
            <div class="collectibles" :style="element.style" :id="element.id"></div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "CollectibleComponent",
        data() {
            return {
                //TODO wyliczyć limit
                howMany: 10,
                collectibles: [],
                width: 10,
                height: 10,
                notToClose: 20
            }
        },
        props: {
            gameAreaData: Object
        },
        watch: {
            gameAreaData: {
                handler() {
                    this.populateCollectibles();
                },
                deep: true
            }
        },
        methods: {
            populateCollectibles() {
                while (this.collectibles.length < this.howMany) {
                    let left = Math.floor(Math.random() * this.gameAreaData.width) - this.width;
                    let top = Math.floor(Math.random() * this.gameAreaData.height) - this.width;
                    if (this.wrongPositions(left, top)) {
                        continue;
                    }
                    let element = {};
                    element.id = `collectible_${left}_${top}`;
                    element.touched = false;
                    element.raw = {
                        left: left,
                        top: top,
                        right: left + this.width,
                        bottom: top + this.height,
                        width: this.width,
                        height: this.height
                    };
                    element.style = {
                        left: left + 'px',
                        top: top + 'px',
                        width: this.width + 'px',
                        height: this.height + 'px'
                    };
                    this.collectibles.push(element);
                }
                this.emitCollectibleData();
            },
            wrongPositions(left, top) {
                if (left < 0 || top < 0) {
                    return true;
                }
                let check = false;
                this.collectibles.forEach(element => {
                    const elementLeft = parseInt(element.left);
                    const elementTop = parseInt(element.top);
                    if (left > (elementLeft - this.notToClose) && left < (elementLeft + this.notToClose * 2) &&
                        top > (elementTop - this.notToClose) && top < (elementTop + this.notToClose * 2)
                    ) {
                        check = true;
                    }
                });
                return check;
            },
            emitCollectibleData() {
                setTimeout(() => {
                    this.$emit('collectibles', this.collectibles);
                }, 800);
            }
        }
    }
</script>

<style scoped>
    .collectibles {
        background-color: red;
        position: absolute;
    }
</style>