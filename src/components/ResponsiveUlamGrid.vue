<template>
    <div class="responsive-ulam-grid" ref="responsivegrid">
        <div class="ulam-grid-wrapper" ref="gridwrapper">
            <ulam-grid
                    v-if="hasMounted"
                    ref="ulamgrid"
                    :numbers="numbers"
                    :start-direction="startDirection"
                    :rotation-direction="rotationDirection"
                    :cell-width="cellWidth"
                    :cell-height="cellHeight"
            />
        </div>
    </div>
</template>

<script>
import UlamGrid from "./UlamGrid";

export default {
    name: 'ResponsiveUlamGrid',
    components: {
        UlamGrid
    },
    props: {
        numbers: {
            type: Array,
            default: function () {
                return [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25];
            }
        },
        startDirection: String,
        rotationDirection: String
    },
    data: function () {
        return {
            sizeChangeEventListener: null,
            hasMounted: false,
            previousWidth: null,
            previousHeight: null
        }
    },
    mounted() {
        this.hasMounted = true;

        this.previousWidth = this.$refs.gridwrapper.clientWidth;

        this.addSizeChangeObserver(this.$refs.gridwrapper);
    },
    unmounted() {
        clearInterval(this.sizeChangeEventListener);
    },
    computed: {
        cellWidth() {
            return (this.previousWidth / ((this.lastCircleAllocatedLength / 4) + 1));
        },
        cellHeight() {
            return this.cellWidth;
        },
        lastCircleAllocatedLength() {
            if (this.numbers.length > 1) {
                return (this.getCirclesCount() - 1) * 8;
            }

            return 1;
        }
    },
    methods: {
        getCirclesCount() {
            let circleCounter = 0;
            let numberCounter = 0;

            this.numbers.forEach((number, index) => {
                numberCounter++;

                if (circleCounter > 0) {
                    if (numberCounter === (8 * circleCounter) || index === this.numbers.length - 1) {
                        circleCounter++;
                        numberCounter = 0;
                    }
                } else {
                    circleCounter++;
                    numberCounter = 0;
                }
            });

            return circleCounter;
        },
        addSizeChangeObserver(refElement) {
            this.sizeChangeEventListener = setInterval(() => {
                if (refElement.clientWidth !== this.previousWidth) {
                    this.previousWidth = refElement.clientWidth;
                }
            }, 250)
        }
    }
}
</script>