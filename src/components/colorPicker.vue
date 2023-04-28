<script setup>
import { ref, watch, defineProps } from 'vue'

const props = defineProps(['updateMainColor'])

const hue = ref(0)

const x = ref('0px')
const y = ref('0px')

const outR = ref('0')
const outG = ref('0')
const outB = ref('0')

function updateCursorLocation(e) {
    const element = document.getElementById('colorPickerMain')
    var rect = element.getBoundingClientRect(); 
    let calculatedX = (e.clientX - rect.left)>=218.4 ? 213.4 : (e.clientX - rect.left)<=0 ? -5 : e.clientX - rect.left - 5
    let calculatedY = (e.clientY - rect.top)>=122.85 ? 118.4 : (e.clientY - rect.top)<=0 ? -5 : e.clientY - rect.top - 5
    if ((e.clientX - rect.left) < -15 || (e.clientX - rect.left) > 230 || (e.clientY - rect.top) < -15 ||(e.clientY - rect.top) > 135) {
        window.removeEventListener('mousemove', updateCursorLocation)
    }
    x.value = `${Math.round(calculatedX)}px`; 
    y.value = `${Math.round(calculatedY)}px`;
    calculateOutput(x.value, y.value);
}
function trackCursor(value) {
    if (value) {
        window.addEventListener('mousemove', updateCursorLocation)
    } else {
        window.removeEventListener('mousemove', updateCursorLocation)
    }
}

watch(hue, async (newHue, oldHue) => {
    calculateOutput(x.value, y.value)
})

function calculateOutput(first, second) {
    let value = 1 - ((parseInt(second.slice(0, -2)) + 5) / 122.85);
    let saturation = (parseInt(first.slice(0, -2)) + 5) / 218.4;
    convertHSVtoRGB(hue.value / 360, saturation, value)
}

function convertHSVtoRGB(h, s, v) {
    var r, g, b, i, f, p, q, t;
    if (arguments.length === 1) {
        s = h.s, v = h.v, h = h.h;
    }
    i = Math.floor(h * 6);
    f = h * 6 - i;
    p = v * (1 - s);
    q = v * (1 - f * s);
    t = v * (1 - (1 - f) * s);
    switch (i % 6) {
        case 0: r = v, g = t, b = p; break;
        case 1: r = q, g = v, b = p; break;
        case 2: r = p, g = v, b = t; break;
        case 3: r = p, g = q, b = v; break;
        case 4: r = t, g = p, b = v; break;
        case 5: r = v, g = p, b = q; break;
    }
    outR.value = Math.round(r * 255)
    outG.value = Math.round(g * 255)
    outB.value = Math.round(b * 255)
    props.updateMainColor(outR.value, outG.value, outB.value)
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
    border: 2px solid white;
    background-color: black;
    position: absolute;
    top: v-bind('y');
    left: v-bind('x');
    z-index: 100;
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
    margin-top: 15px;
    background-color: rgba(v-bind('outR'), v-bind('outG'), v-bind('outB'), 1);
    height: 25px;
    width: 100%;
}
</style>