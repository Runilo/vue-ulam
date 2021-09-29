<template>
    <div class="ulam-grid-cell"
         :class="[(isPrime ? 'prime': '')]"
         v-html="ulamNumber"
         :style="{width: width + sizeUnit, height: height + sizeUnit, fontSize: fontSize + sizeUnit}"
         ref="cell"
    >

    </div>
</template>

<script>
export default {
    name: 'UlamGridCell',
    props: {
        ulamNumber: Number,
        cellWidth: Number,
        cellHeight: Number,
        sizeUnit: String,
        gridWidth: Number
    },
    computed: {
        fontSize() {
            let strLength = this.ulamNumber.toString().length;

            if (strLength > 1) {
                return (this.cellHeight / strLength) * 1.5;
            }

            return this.cellHeight;
        },
        isPrime() {
            return this.checkIfPrime(this.ulamNumber);
        },
        width() {
            return this.cellWidth;
        },
        height() {
            return this.cellHeight;
        }
    },
    methods: {
        checkIfPrime(n) {
            if (isNaN(n) || !isFinite(n) || n % 1 || n < 2) return false;
            if (n % 2 === 0) return (n === 2);
            var m = Math.sqrt(n);
            for (var i = 3; i <= m; i += 2) {
                if (n % i === 0) return false;
            }
            return true;
        }
    }
}
</script>

<style>
.ulam-grid-cell {
    text-align: center;
    position: absolute;
}
</style>