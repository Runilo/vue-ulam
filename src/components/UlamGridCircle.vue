<template>
    <div class="circle-wrapper" :style="{top: top + sizeUnit, left: left + sizeUnit, zIndex: zIndex}">
        <div class="ulam-grid-circle" :class="[rotationDirection]"
             :style="{width: width + sizeUnit, height: height + sizeUnit}">
            <ulam-grid-radian
                    v-for="(radian, index) in circleRadians"
                    :key="index" :numbers="radian"
                    :radian-index="index"
                    :rotation-direction="rotationDirection"
                    :start-direction="startDirection"
                    :cell-width="cellWidth"
                    :cell-height="cellHeight"
                    :size-unit="sizeUnit"
                    :allocated-radian-length="allocatedRadianLength"
                    :grid-width="gridWidth"
            />
        </div>
    </div>
</template>

<script>
import UlamGridRadian from './UlamGridRadian.vue';

export default {
    name: 'UlamGridCircle',
    props: {
        numbers: Array,
        rotationDirection: String,
        startDirection: String,
        cellWidth: Number,
        cellHeight: Number,
        sizeUnit: String,
        zIndex: Number,
        width: Number,
        height: Number,
        top: Number,
        left: Number,
        allocatedCircleLength: Number,
        fontSizeDecreasePercentage: String,
        gridWidth: Number
    },
    components: {
        UlamGridRadian
    },
    computed: {
        circleRadians() {
            return this.groupNumbersIntoRadians();
        },
        allocatedRadianLength() {
            if (this.allocatedCircleLength > 1) {
                return this.allocatedCircleLength / 4;
            }

            return 1;
        }
    },
    methods: {
        groupNumbersIntoRadians() {
            let thisRadians = [];
            let tempRadian = [];

            this.numbers.forEach((number, index) => {
                // Group circle numbers in to 4 radians
                // If this circle holds 8 numbers, [1,2,3,4,5,6,7,8], then this if will catch [[1,2],[3,4],[5,6]]
                if (index > 0 && (index % this.allocatedRadianLength === 0)) {
                    thisRadians.push(tempRadian);
                    tempRadian = [];
                }

                tempRadian.push(number);

                // Catch the last radian, [7,8], or the last bit of incomplete circle
                if (this.numbers.length - 1 === index) {
                    thisRadians.push(tempRadian);
                }
            });

            return thisRadians;
        }
    }
}
</script>

<style>
.circle-wrapper {
    position: absolute;
}

.ulam-grid-circle {
    position: relative;
}

.ulam-grid-circle.clockwise {
    text-align: right;
}

.ulam-grid-circle.counterclockwise {
    text-align: left;
}
</style>