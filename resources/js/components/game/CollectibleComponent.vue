<template>
    <div>
        <div v-for="element in collectibles">
            <div class="collectibles" :style="element.style" :id="element.id"></div>
        </div>
    </div>
</template>

<script>
    const HOW_MANY = 10;
    const GEMS = [
        {
            'background-color': 'gold',
            quantity: Math.floor(HOW_MANY / HOW_MANY),
            points: 10
        },
        {
            'background-color': 'red',
            quantity: Math.floor(HOW_MANY / 2),
            points: 2
        },
        {
            'background-color': 'blue',
            quantity: Math.floor(HOW_MANY / 4),
            points: 5
        },
        {
            'background-color': 'green',
            quantity: HOW_MANY, //'rest'
            points: 1
        }

    ];
    export default {
        name: "CollectibleComponent",
        data() {
            return {
                //TODO wyliczyć limit
                howMany: HOW_MANY,
                collectibles: [],
                width: 10,
                height: 10,
                notToClose: 20,
                gems: GEMS
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
                        height: this.height + 'px',
                };
                    this.collectibles.push(element);
                }
                this.createGems();
                this.emitCollectibleData();
            },
            createGems() {
                let counter = 0;
                this.gems.forEach((gem) => {
                    counter += gem.quantity;
                    this.collectibles.forEach((collectible, index) => {
                        if ((index + 1) <= counter && !collectible.changed) {
                            collectible.changed = true;
                            collectible.points = gem.points;
                            collectible.style['background-color'] = gem['background-color'];
                        }
                    });
                });
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
        position: absolute;
    }
</style>