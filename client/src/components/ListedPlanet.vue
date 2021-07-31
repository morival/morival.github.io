<template>
    <div class="listed-planet">
        <label 
            class="img-container"
            :id="planet.englishName">
                <input 
                    :id="`${planet.englishName}`" 
                    type="checkbox"  
                    :value="`${planet.englishName}`" 
                    v-model="planetChecked" 
                    v-on:click="handleClick()"/>
                <img
                    v-bind:class="isActive"
                    v-bind:src="require(`../assets/images/${planet.englishName}.png`)" 
                    :title="`${planet.englishName}`" alt="picture of chosen planet" 
                    :width="setWidthOfPlanetImage()"/>
        </label>
    </div>
        <!-- <div 
            class="listed-planet"
            v-bind:class="showShadow"
            v-on:click="handleClick()">
            <label 
                class="img-container"
                :id="planet.englishName">
                <input 
                    :id="`${planet.englishName}`" 
                    type="checkbox"  
                    :value="`${planet.englishName}`" 
                    v-model="planetChecked" 
                    v-on:click="handleClick()"/>
                <img
                    v-bind:class="isActive"
                    v-bind:src="require(`../assets/images/${planet.englishName}.png`)" 
                    :title="`${planet.englishName}`" alt="picture of chosen planet" 
                    :width="setWidthOfPlanetImage()"/>
            </label>
        </div> -->
</template>

<script>
import {eventBus} from '@/main.js';

export default {
    name: 'listed-planet',
    props: ['planet', 'isActive'
    // , 'showShadow'
    ],
    data() {
        return {
            widthOfImage: 70,
            planetChecked: [],
            // isActive: null
        }
    },
    mounted() {
        // eventBus.$on('planet-by-size', activate => {
        //     this.isActive = activate;
        // })
    },
    methods: {
        handleClick: function() {
            eventBus.$emit('planet-selected', this.planet);
        },
        setWidthOfPlanetImage() {
            let number = (this.planet.meanRadius / 450);
                return number + 'px'
        },
    },
}
</script>

<style>
/* HIDE RADIO */
[type=checkbox] { 
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

/* IMAGE STYLES */
[type=checkbox] + img {
  cursor: pointer;
}

/* CHECKED STYLES */

.img-container {
    position: flex;
}
/* #Saturn img {
    width: 200px;
    max-width: 200px;
    margin-bottom: -5px;
    margin-left: -45px;
} */
.listed-planet > label {
    width: 110px;
    text-align: center;
}
.distance, .density, .gravity {
    width: 100px;
    max-width: 100px;
    transition-duration: 0.75s;
}
.size {
    transition-duration: 0.75s;
}
/* .showShadow {
    background-color: black;
    border-radius: 100%;
    width: 55px;
    height: 55px;
    position: flex;
    justify-content: center;
    align-items: center;
} */
.listed-planet {
    color: white;
    /* margin: 10px; */
    font-size: 20px;
    display: flex;
    flex-direction: row;
}
.listed-planet > label > img:hover {
    transition-timing-function: ease;
    transform: scale(1.8, 1.8);
}
.listed-planet > #Saturn > img:hover {
    transition-timing-function: ease;
    transform: scale(1.5, 1.5);
}
/* .showShadow > #Mercury > img:hover {
    transition-timing-function: ease;
    transform: scale(35, 35);
}
.showShadow > #Mars > img:hover {
    transition-timing-function: ease;
    transform: scale(27, 27);
}
.showShadow > #Venus > img:hover {
    transition-timing-function: ease;
    transform: scale(14, 14);
}
.showShadow > #Earth > img:hover {
    transition-timing-function: ease;
    transform: scale(12.8, 12.8);
}
.showShadow > #Neptune > img:hover {
    transition-timing-function: ease;
    transform: scale(3, 3);
}
.showShadow > #Uranus > img:hover {
    transition-timing-function: ease;
    transform: scale(3, 3);
}
.showShadow > #Saturn > img:hover {
    transition-timing-function: ease;
    transform: scale(1.7, 1.7);
}
.showShadow > #Jupiter > img:hover {
    transition-timing-function: ease;
    transform: scale(1.2, 1.2);
} */


</style>
