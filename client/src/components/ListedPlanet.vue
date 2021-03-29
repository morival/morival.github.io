<template>
        <div class="listed-planet">
            <label :id="planet.englishName">
                <input :id="`${planet.englishName}`" type="checkbox"  
                :value="`${planet.englishName}`" v-model="planetChecked" v-on:click="handleClick()"/>
                <img
                    v-bind:class="isActive"
                    v-bind:src="require(`../assets/images/${planet.englishName}.png`)" 
                    :title="`${planet.englishName}`" alt="picture of chosen planet" 
                    :width="setWidthOfPlanetImage()"/>
            </label>

        </div>
</template>

<script>
import {eventBus} from '@/main.js';

export default {
    name: 'listed-planet',
    props: ['planet', 'isActive'],
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
            let number = (this.planet.meanRadius / 500);
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


#Saturn img {
    width: 200px;
    max-width: 200px;
    margin-bottom: -25px;
    margin-left: -60px;
}
.listed-planet > label {
    width: 110px;
    text-align: center;
}
.distance {
    width: 100px;
    max-width: 100px;
    transition-duration: 0.75s;
}
.size {
    transition-duration: 0.75s;
}
.density {
    width: 100px;
    max-width: 100px;
    transition-duration: 0.75s;
}
.gravity {
    width: 100px;
    max-width: 100px;
    transition-duration: 0.75s;
}
.listed-planet > label > img:hover{
    transition-timing-function: ease;
    /* width: 250px;
    height: 250px; */
    transform: scale(2, 2);
    /* opacity: 100%; */
}


.listed-planet {
    color: white;
    margin: 10px;
    font-size: 20px;
    display: flex;
    flex-direction: row;
    
    background-color: black;
    border-radius: 100%;
    
}

</style>
