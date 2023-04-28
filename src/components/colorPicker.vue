<script setup>
import { ref, reactive } from 'vue'

const hue = ref(0)

const x = ref('0px')
const y = ref('0px')

const r = ref('0')
const g = ref('0')
const b = ref('0')

function updateCursorLocation(e) {
    const element = document.getElementById('colorPickerMain')
    var rect = element.getBoundingClientRect(); 
    x.value = `${(e.clientX - rect.left)>=218.4 ? 213.4 : (e.clientX - rect.left)<=0 ? -5 : e.clientX - rect.left - 5}px`; 
    y.value = `${(e.clientY - rect.top)>=122.85 ? 118.4 : (e.clientY - rect.top)<=0 ? -5 : e.clientY - rect.top - 5}px`;
    calculateOutput(x.value, y.value);
    console.log('Cursor position: ' + x.value + ',' + y.value); 
}
function trackCursor(value) {
    if (value) {
        window.addEventListener('mousemove', updateCursorLocation)
    } else {
        window.removeEventListener('mousemove', updateCursorLocation)
    }
}

function calculateOutput(first, second) {
    first.slice(0, -1); 
    second.slice(0, -1); 
    let value = (parseInt(first) + 5) / 218.4;
    let saturation = (parseInt(second) + 5) / 122.85;
    convertHSVtoRGB(hue.value, saturation, value)
}

function convertHSVtoRGB(h, s, v) {
    if (arguments.length == 1) {
        var { h, s, v } = arguments[0];
    }

    var H = h;
    var S = s;
    var V = v;

    if (H == 360) {
        H = 0;
    }

    const C = S * V;
    const X = C * (1 - Math.abs((H / 60) % 2 - 1));
    const m = V - C;

    let temp = [];

    if (0 <= H && H < 60) { temp = [C, X, 0]; }
    else if (60 <= H && H < 120) { temp = [X, C, 0]; }
    else if (120 <= H && H < 180) { temp = [0, C, X]; }
    else if (180 <= H && H < 240) { temp = [0, X, C]; }
    else if (240 <= H && H < 300) { temp = [X, 0, C]; }
    else if (300 <= H && H < 360) { temp = [C, 0, X]; }

    r.value = Math.round((temp[0] + m) * 255)
    g.value = Math.round((temp[1] + m) * 255)
    b.value = Math.round((temp[2] + m) * 255)
    console.log(Math.round((temp[0] + m) * 255), g.value, b.value)
}
</script>
<template>
    <div id="colorPickerHero" @mouseup="() => trackCursor(false)">
        <div id="colorPickerMain" @mousedown="() => trackCursor(true)" @mouseup="() => trackCursor(false)" @click="(e) => updateCursorLocation(e)">
            <div id="selector"></div>
            <div id="saturation"></div>
            <div id="value"></div>
        </div>
        <div id="hueSlider">
            <input type="range" min="1" max="360" step="1" class="slider" id="hueSliderRange" v-model="hue">
        </div>
        <div id="transparencySlider"></div>
        <div id="output"></div>
    </div>
</template>
<style scoped>
#colorPickerHero {
    padding: 15px;
    width: 250px;
    height: 300px;
    border: 1px solid rgb(174, 174, 174);
}
#colorPickerMain {
    position: relative;
    width: 100%;
    height: auto;
    aspect-ratio: 16/9;
}
#selector {
    height: 10px;
    width: 10px;
    border-radius: 50px;
    color: white;
    border: 1px solid grey;
    position: absolute;
    top: v-bind('y');
    left: v-bind('x');
}
#saturation {
    position: absolute;
    height: 100%;
    width: 100%;
    background-image: linear-gradient(270deg, hsla(v-bind('hue'), 100%, 50%, 1),hsla(0, 0%, 0%, 0));
}
#value {
    position: relative;
    height: 100%;
    width: 100%;
    background-image: linear-gradient(0deg, hsla(v-bind('hue'), 0%, 0%, 1), hsla(v-bind('hue'), 100%, 50%, 0));
}
#hueSlider {
    margin-top: 15px;
    width: 100%;
    height: 25px;
    border: 1px solid grey;
    border-left: 0px;
    border-right: 0px;
}
#hueSliderRange {
    padding: 0px;
    width: 100%;
    height: 23px;
    background: linear-gradient(to right, #ff0000 0%, #ffff00 17%, #00ff00 33%, #00ffff 50%, #0000ff 67%, #ff00ff 83%, #ff0000 100%);
    background-origin: content-box;
    appearance: none;
    -webkit-appearance: none;
}
#hueSliderRange::-webkit-slider-thumb, #hueSliderRange::-moz-range-thumb {
    height: 27.5px;
    width: 5px;
    -webkit-appearance: none;
}
#output {
    color: rgba(v-bind('r'), v-bind('g'), v-bind('b'), 1);
    height: 25px;
    width: 100%;
}
</style>