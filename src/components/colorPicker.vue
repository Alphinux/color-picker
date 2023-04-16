<script setup>
import { ref } from 'vue'

const hue = ref(0)
</script>
<template>
    <div id="colorPickerHero">
        <div id="colorPickerMain">
            <div id="saturation">
                <div id="saturation-blend"></div>
            </div>
            <div id="value">
                <div id="value-blend"></div>
            </div>
        </div>
        <div id="hueSlider">
            <input type="range" min="1" max="360" step="1" class="slider" id="hueSliderRange" v-model="hue">
        </div>
        <div id="transparencySlider"></div>
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
#saturation {
    position: absolute;
    opacity: 0;
    height: 100%;
    width: 100%;
    mask-image: linear-gradient(45deg, rgba(0, 0, 0, 1), transparent);
    background-image: linear-gradient(270deg, hsla(v-bind('hue'), 100%, 50%, 1),hsla(0, 0%, 0%, 0));
}
#value {
    position: relative;
    opacity: 0;
    height: 100%;
    width: 100%;
    mask-image: linear-gradient(180deg, transparent, rgba(0, 0, 0, 1));
    background-image: linear-gradient(0deg, hsla(v-bind('hue'), 0%, 0%, 1), hsla(v-bind('hue'), 100%, 50%, 1));
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
</style>