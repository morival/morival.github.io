<template>
    <div id="selected-planet" v-if="planet.isPlanet">
        <div class="planet-container">
            <div>
                <h2>Planet {{planet.englishName}} </h2>
                <spinning-globes :planet="planet"></spinning-globes>
            </div>
            <div id="listed-planet-details">
                
                <div v-for="(body, index) in descriptions" :key="index">
                    <h5 v-if="body.name === planet.englishName">Description:</h5>
                    <p v-if="body.name === planet.englishName"> {{ body.definition }}</p>
                </div>
                <h5>Specification:</h5>
                <p>Average Distance from Sun: 
                    <span class="spec-value" v-if="convertDistance"> {{planet.semimajorAxis}} km</span>
                    <span class="spec-value" v-else> {{milesConvertor(planet.semimajorAxis)}} miles</span>
                </p>
                <p>Time to Orbit Sun (a year):
                    <span class="spec-value"> {{planet.sideralOrbit}} days</span>
                </p>
                <p>Time to Spin on Axis (a day):
                    <span class="spec-value"> {{Math.round(planet.sideralRotation)}} hours</span>
                </p>
                <p>Average Radius: 
                    <span class="spec-value" v-if="convertDistance"> {{Math.round(planet.meanRadius)}} km</span>
                    <span class="spec-value" v-else> {{milesConvertor(planet.meanRadius)}} miles</span>
                </p>
                <p>Gravity:
                    <span class="spec-value"> {{planet.gravity}} m/s²</span>
                </p>
                <p>Density:
                    <span class="spec-value"> {{planet.density}} g/cm³</span>
                </p>
                <p>Escape Velocity: 
                    <span class="spec-value" v-if="convertDistance"> {{(planet.escape)/1000}} km/s</span>
                    <span class="spec-value" v-else> {{milesConvertor(planet.escape)/1000}} mi/s</span>
                </p>
                <button @click="convertDistance = !convertDistance" class="btn">Convert to miles</button>
            </div>
        </div>
        
        <moon-list v-if="getMoons" :getMoons="getMoons"></moon-list>
    </div>
</template>

<script>
import MoonList from './MoonList.vue'
import SpinningGlobes from './SpinningGlobes.vue'



export default {
    name: 'planet-detail',
    props: ['planet', 'getMoons', 'moons', 'descriptions', 'planets'],
    data() {
        return {
            convertDistance: true
        }
    },
    components: {
        'moon-list': MoonList,
        'spinning-globes': SpinningGlobes
    },
    methods: {
        milesConvertor: function(number){
            return Math.round(number/ 1.609)
        }
    }

}
</script>

<style>
#selected-planet {
    margin-top: 30px;
    min-width: 1020px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    background-color: RGBA(255, 105, 180, 0.1);
    padding: 10px;
    box-shadow: 5px 10px rgba(255, 255, 255, 0.342);
    border-radius: 6px;
    margin-bottom: 10px;
    padding-bottom: 10px;

}

#listed-planet-details {
    margin-left: 30px;
    margin-top: 20px;
    width: 500px;
}
.planet-container {
    display: flex;
    /* z-index: 1; */
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
.spec-value {
    color:khaki;
}


</style>
