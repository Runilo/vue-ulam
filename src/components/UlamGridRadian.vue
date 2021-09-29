<template>
    <div
            class="ulam-cirlce-radian"
            :class="[radianFirstBorder, radianSecondBorder]"
            :style="{width: width + sizeUnit, height: height + sizeUnit}"
    >
        <div class="radian-inner"
             :style="{width: width + sizeUnit, height: height + sizeUnit, fontSize: fontSize, top: top + sizeUnit, left: left + sizeUnit}">
            <ulam-grid-cell
                    v-for="(item, index) in radianNumbers"
                    :ulam-number="item"
                    :key="index"
                    :cell-width="cellWidth"
                    :cell-height="cellHeight"
                    :size-unit="sizeUnit"
                    :style="{top: getCellTop(index) + sizeUnit, left: getCellLeft(index) + sizeUnit}"
                    :grid-width="gridWidth"
            />
        </div>
    </div>
</template>

<script>
import UlamGridCell from './UlamGridCell.vue';

export default {
    name: 'UlamGridRadian',
    components: {
        UlamGridCell
    },
    props: {
        numbers: Array,
        radianIndex: Number,
        rotationDirection: String,
        startDirection: String,
        cellWidth: Number,
        cellHeight: Number,
        sizeUnit: String,
        allocatedRadianLength: Number,
        gridWidth: Number
    },
    data() {
        return {
            bordersClockwise: ['top', 'right', 'bottom', 'left'],
            bordersCounterClockwise: ['top', 'left', 'bottom', 'right'],
            innerTop: 0,
            innerLeft: 0
        }
    },
    computed: {
        fontSize() {
            return this.cellHeight + this.sizeUnit;
        },
        top() {
            let diff = this.allocatedRadianLength - this.numbers.length;

            if (diff > 0 && this.checkIfReversed()) {
                if (this.height !== this.cellHeight) {
                    return diff * this.cellHeight;
                }
            }

            return 0;
        },
        left() {
            let diff = this.allocatedRadianLength - this.numbers.length;

            if (diff > 0 && this.checkIfReversed()) {
                if (this.width !== this.cellWidth) {
                    return diff * this.cellWidth;
                }
            }

            return 0;
        },
        radianNumbers() {
            // Reverse the numbers in the radian if needed
            if (this.checkIfReversed()) {
                let diff = this.allocatedRadianLength - this.numbers.length;

                if (diff > 0) {
                    if (this.width === this.cellWidth) {
                        console.log('top:', diff * this.cellHeight);
                    } else {
                        console.log('left:', diff * this.cellWidth);
                    }
                }

                return this.numbers.slice().reverse();
            }

            return this.numbers;
        },
        radianFirstBorder() {
            return this.sortedBorders[this.radianIndex];
        },
        radianSecondBorder() {
            if (this.radianIndex < 3) {
                return this.sortedBorders[this.radianIndex + 1];
            }

            return this.sortedBorders[0];
        },
        sortedBorders() {
            if (this.rotationDirection === 'counterclockwise') {
                return this.rotateArray(this.bordersCounterClockwise, this.bordersCounterClockwise.indexOf(this.startDirection));
            }

            return this.rotateArray(this.bordersClockwise, this.bordersClockwise.indexOf(this.startDirection));
        },
        width() {
            if (this.radianFirstBorder === 'left' || this.radianFirstBorder === 'right') {
                return this.cellWidth;
            }

            return this.cellWidth * this.allocatedRadianLength;
        },
        height() {
            if (this.radianFirstBorder === 'left' || this.radianFirstBorder === 'right') {
                return this.cellHeight * this.allocatedRadianLength;
            }

            return this.cellHeight;
        }
    },
    methods: {
        checkIfReversed: function () {
            let thisBorders = [this.radianFirstBorder, this.radianSecondBorder];

            if (this.rotationDirection === 'counterclockwise') {
                if (thisBorders.indexOf('top') !== -1 && (thisBorders.indexOf('left') !== -1 || thisBorders.indexOf('right') !== -1)) {
                    return true;
                }
            } else {
                if (thisBorders.indexOf('left') !== -1 && (thisBorders.indexOf('top') !== -1 || thisBorders.indexOf('bottom') !== -1)) {
                    return true;
                }
            }

            return false;
        },
        rotateArray: function (array, n) {
            n = n % array.length;
            while (array.length && n < 0) n += array.length;
            array.push.apply(array, array.splice(0, n));
            return array;
        },
        getCellTop(cellIndex) {
            if (this.radianFirstBorder === 'left' || this.radianFirstBorder === 'right') {
                return this.cellHeight * cellIndex;
            }

            return 0;
        },
        getCellLeft(cellIndex) {
            if (this.radianFirstBorder === 'left' || this.radianFirstBorder === 'right') {
                return 0;
            }

            return this.cellWidth * cellIndex;
        }
    }
}
</script>

<style>
.ulam-cirlce-radian {
    position: absolute;
    display: inline-block;
}

.ulam-cirlce-radian.top {
    top: 0;
}

.ulam-cirlce-radian.right {
    right: 0;
}

.ulam-cirlce-radian.bottom {
    bottom: 0;
}

.ulam-cirlce-radian.left {
    left: 0;
}

.radian-inner {
    position: relative;
}
</style>