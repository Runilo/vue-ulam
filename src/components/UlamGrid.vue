<template>
    <div class="ulam-grid"
         :style="{
            width: getCircleWidth(ulamCircles.length - 1) + sizeUnit,
            height: getCircleHeight(ulamCircles.length - 1) + sizeUnit,
            lineHeight: cellHeight + sizeUnit
         }"
         ref="grid"
    >
        <ulam-grid-circle
                v-for="(circle, circleIndex) in ulamCircles"
                :numbers="circle"
                :key="circleIndex"
                :cell-width="cellWidth"
                :cell-height="cellHeight"
                :size-unit="sizeUnit"
                :start-direction="startDirection"
                :rotation-direction="rotationDirection"
                :z-index="this.ulamCircles.length - 1 - circleIndex"
                :width="getCircleWidth(circleIndex)"
                :height="getCircleHeight(circleIndex)"
                :top="((ulamCircles.length - 1) - circleIndex) * this.cellHeight"
                :left="((ulamCircles.length - 1) - circleIndex) * this.cellWidth"
                :allocated-circle-length="getAllocatedCircleLength(circleIndex)"
                :grid-width="this.gridWidth"
        />
    </div>
</template>

<script>
import UlamGridCircle from './UlamGridCircle.vue';

export default {
    name: 'UlamGrid',
    props: {
        numbers: Array,
        startDirection: {
            type: String,
            default: 'right',
            validator: function (value) {
                return ['top', 'right', 'bottom', 'left'].indexOf(value) !== -1
            }
        },
        rotationDirection: {
            type: String,
            default: 'counterclockwise',
            validator: function (value) {
                return ['clockwise', 'counterclockwise'].indexOf(value) !== -1
            }
        },
        cellWidth: {
            type: Number,
            default: 20
        },
        cellHeight: {
            type: Number,
            default: 20
        },
        sizeUnit: {
            type: String,
            default: 'px'
        }
    },
    components: {
        UlamGridCircle
    },
    mounted() {
        this.gridWidth = this.$refs.grid.clientWidth;
    },
    data() {
        return {
            gridWidth: undefined
        }
    },
    computed: {
        ulamCircles() {
            return this.numbersToCircles(this.numbers);
        }
    },
    methods: {
        getAllocatedCircleLength(circleIndex) {
            if (circleIndex > 0) {
                return circleIndex * 8;
            }

            return 1;
        },
        getCircleWidth(circleIndex) {
            if (circleIndex > 0) {
                return (((circleIndex * 8) / 4) + 1) * this.cellWidth;
            }

            return this.cellWidth;
        },
        getCircleHeight(circleIndex) {
            if (circleIndex > 0) {
                return (((circleIndex * 8) / 4) + 1) * this.cellHeight;
            }

            return this.cellHeight;
        },
        numbersToCircles(arrayOfNumbers) {
            const circles = [];
            let tempCircle = [];

            arrayOfNumbers.forEach((number, index) => {
                tempCircle.push(number);

                if (circles.length > 0) {
                    if (tempCircle.length === (8 * circles.length) || index === arrayOfNumbers.length - 1) {
                        circles.push(tempCircle);
                        tempCircle = [];
                    }
                } else {
                    circles.push(tempCircle);
                    tempCircle = [];
                }
            });

            return circles;
        }
    }
}
</script>

<style scoped>
.ulam-grid {
    position: relative;
}
</style>