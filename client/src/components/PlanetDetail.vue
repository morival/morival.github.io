<template>
    <div class="details-container" v-if="planet.isPlanet">
        <div class="main-details">
            <div class="logo-details">
                <h2>Planet {{planet.englishName}} </h2>
                <spinning-globes :planet="planet"></spinning-globes>
            </div>
            <div class="spec-details">
                <div v-for="(body, index) in descriptions" :key="index">
                    <h5 v-if="body.name === planet.englishName">Description:</h5>
                    <p v-if="body.name === planet.englishName"> {{body.definition}}</p>
                </div>
                <h5>Specification:</h5>
                <p>Average Distance from Sun: 
                    <span class="khaki" v-if="convertDistance"> {{planet.semimajorAxis}} km</span>
                    <span class="khaki" v-else> {{milesConvertor({props: planet.semimajorAxis})}} miles</span>
                </p>
                <p>Time to Orbit Sun (a year):
                    <span class="khaki"> {{planet.sideralOrbit}} days</span>
                </p>
                <p>Time to Spin on Axis (a day):
                    <span class="khaki"> {{Math.round(planet.sideralRotation)}} hours</span>
                </p>
                <p>Average Radius: 
                    <span class="khaki" v-if="convertDistance"> {{Math.round(planet.meanRadius)}} km</span>
                    <span class="khaki" v-else> {{milesConvertor({props: planet.meanRadius})}} miles</span>
                </p>
                <p>Gravity:
                    <span class="khaki"> {{planet.gravity}} m/s²</span>
                </p>
                <p>Density:
                    <span class="khaki"> {{planet.density}} g/cm³</span>
                </p>
                <p>Escape Velocity: 
                    <span class="khaki" v-if="convertDistance"> {{(planet.escape)/1000}} km/s</span>
                    <span class="khaki" v-else> {{milesConvertor({props: planet.escape})/1000}} mi/s</span>
                </p>
                <button @click="convertDistance = !convertDistance" class="btn">Convert to miles</button>
            </div>
        </div>
        <moon-list 
        v-if="getMoons" 
        :getMoons="getMoons"/>
        <div class="moon-list-slot" v-else/>
    </div>
</template>

<script>
import MoonList from './MoonList.vue'
import SpinningGlobes from './SpinningGlobes.vue'



export default {
    name: 'planet-detail',
    props: ['planet', 'getMoons', 'moons', 'descriptions', 'planets', 'milesConvertor'],
    data() {
        return {
            convertDistance: true
        }
    },
    components: {
        'moon-list': MoonList,
        'spinning-globes': SpinningGlobes
    }
}
</script>

<style>
.details-container {
    margin: 30px 0;
    padding: 10px;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    background-color: RGBA(255, 105, 180, 0.1);
    box-shadow: 5px 10px rgba(255, 255, 255, 0.342);
    border-radius: 6px;
}

.logo-details {
    padding: 0 30px;
}

.spec-details {
    /* margin-left: 30px; */
    margin-top: 20px;
    /* width: 500px; */
}
.main-details {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    /* max-width: 650px; */
    /* z-index: 1; */
}
.moon-list-slot {
    width: 320px;
}
p {
    font-family: 'VT323', monospace;
    font-size: 2;
    border: 0;
    margin: 0;
    padding-top: 20px;
    color: white;
    text-shadow: 3px 3px 5px black;
}

h2 {
    color: white;
    text-align: center;
    text-shadow: 5px 5px 8px black;
}
h3 {
    color: white;
    margin-bottom: 0;
    text-shadow: 5px 5px 8px black;
}
h5 {
    color: white;
    margin-bottom: 0;
    text-shadow: 3px 3px 5px black;
}


</style>
