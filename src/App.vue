<template>
    <div class="ulam-grid-demo">
        <h1>Lava</h1>
        <div id="controls">
            <form id="ulam-controls" name="ulam-controls">
                <div class="group">
                    <div class="start form-group">
                        <label for="start">Start number</label>

                        <input class="form-control form-control" type="number" id="start" name="start"
                               v-model.number="start">
                    </div>

                    <div class="length form-group">
                        <label for="length">Length</label>

                        <input class="form-control form-control" type="number" id="length" name="length"
                               v-model.number="length">
                    </div>
                </div>

                <div class="group">
                    <div class="start-direction form-group">
                        <label for="start-direction">Start direction</label>

                        <select class="form-control form-control" name="start-direction" id="start-direction"
                                v-model="startDirection">
                            <option value="top">Top</option>
                            <option value="right">Right</option>
                            <option value="bottom">Bottom</option>
                            <option value="left">Left</option>
                        </select>
                    </div>

                    <div class="rotation-direction form-group">
                        <label for="rotation-direction">Rotation direction</label>

                        <select class="form-control form-control-lg" name="rotation-direction" id="rotation-direction"
                                v-model="rotationDirection">
                            <option value="clockwise">Clockwise</option>
                            <option value="counterclockwise">Counter clockwise</option>
                        </select>
                    </div>
                </div>
            </form>
        </div>
    </div>

    <responsive-ulam-grid
            :numbers="numbers"
            :start-direction="startDirection"
            :rotation-direction="rotationDirection"
    />
</template>

<script>
import "bootstrap/dist/css/bootstrap.min.css";
import ResponsiveUlamGrid from "./components/ResponsiveUlamGrid";

export default {
    name: 'App',
    data() {
        return {
            start: 1,
            length: 25,
            startDirection: 'right',
            rotationDirection: 'counterclockwise'
        };
    },
    components: {
        ResponsiveUlamGrid
    },
    computed: {
        numbers() {
            return this.rangeToNumbers(this.start, this.length);
        }
    },
    methods: {
        rangeToNumbers(start, length) {
            let numbers = [];

            for (let i = 0; i < length; i++) {
                numbers.push(i + start);
            }

            return numbers;
        }
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}

h1 {
    font-size: 90px;
    font-weight: bold;
    font-family: monospace;
    margin: 20px 0;
}

#controls {
    margin: 0 auto;
    text-align: center;
}

.group {
    margin-bottom: 10px;
    display: inline-block;
    margin-right: 20px;
}

.form-group {
    margin-bottom: 5px;
}

label {
    font-size: 12px;
}

.ulam-grid-wrapper {
    margin: 0 20px;
}

.responsive-ulam-grid {
    margin: 0 auto;
    max-width: 500px;
}

.ulam-grid-circle {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}

.ulam-grid-cell.prime {
    color: red;
    font-weight: bold;
}
</style>
