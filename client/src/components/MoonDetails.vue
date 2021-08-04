<template>
  <div class="details-container">
    <div class="main-details">
      <div class="logo-details">
        <h2 v-if="moonName(moon) != 'Moon'">{{ moonName(moon) }} moon</h2>
        <h2 v-else>Moon</h2>
        <img v-bind:src="require(`../assets/images/Moon.png`)" title="picture" alt="picture of chosen moon" height="300px" />
      </div>
      <div class="spec-details">
        <h5>Specification:</h5>
        <p>Orbits:
          <span class="khaki"> {{ showAroundPlanet(moon) }} </span>
        </p>

        <div v-if="moon.semimajorAxis != 0">
          <p>Average Distance from Sun:
            <span class="khaki" v-if="convertDistance"> {{moon.semimajorAxis}} km</span>
            <span class="khaki" v-else> {{milesConvertor({props: moon.semimajorAxis})}} miles</span>
          </p>
        </div>
        <div v-else><p class="red"> Average Distance from Sun Not Available </p></div>

        <div v-if="moon.sideralOrbit != 0">
          <p>Time to Orbit the Planet:
            <span class="khaki"> {{moon.sideralOrbit}} days </span>
          </p>
        </div>
        <div v-else><p class="red">Time to Orbit the Planet Not Available</p></div>

        <div v-if="moon.sideralRotation != 0">
          <p>Time to Spin on Axis (a day):
            <span class="khaki">  {{Math.round(moon.sideralRotation)}} hours </span>
          </p>
        </div>
        <div v-else><p class="red">Time to Spin on Axis Not Available</p></div>

        <div v-if="moon.equaRadius != 0">
          <p> Average Radius:
            <span class="khaki" v-if="convertDistance"> {{Math.round(moon.equaRadius)}} km </span>
            <span class="khaki" v-else> {{milesConvertor({props: moon.equaRadius})}} miles</span>
          </p>
        </div>
        <div v-else><p class="red">Average Radius Not Available</p></div>

        <div v-if="moon.gravity != 0">
          <p>Gravity: 
            <span class="khaki"> {{ moon.gravity }}m/s² </span>
          </p>
        </div>
        <div v-else><p class="red">Gravity Not Available</p></div>
        <button @click="convertDistance = !convertDistance" class="btn">Convert to miles</button>
      </div>
    </div>
    <div>
      <button @click="returnPlanet(moon)" class="btn">Back to {{ showAroundPlanet(moon) }}</button>
    </div>
  </div>
</template>

<script>
  import {eventBus} from '@/main';

  export default {
    name: 'moon-detail',
    props: ['moon', 'isSelected', 'planets', 'milesConvertor'],
    data() {
        return {
            convertDistance: true
        }
    },
    computed: {

    },
    methods: {
      moonName: function(moon){
        if (moon.englishName !== "") {
          return moon.englishName
        } else {
          return moon.name
        }
      },
      returnPlanet: function (moon){
        let orbitsPlanet = moon.aroundPlanet.rel;
        eventBus.$emit('return-planet', orbitsPlanet);
      },
      showAroundPlanet: function (moon){
        let orbitsPlanetRel = moon.aroundPlanet.rel;
        let orbitsPlanet = this.planets.find(planet => planet.rel === orbitsPlanetRel);
        return orbitsPlanet.englishName;
      }
    },
  }
</script>

<style>
.selected-moon {
  margin-top: 30px;
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

.moon-container {
  display: flex;
}
.moon-label {
  width: 310px;
}
.listed-moon-details {
  margin-left: 30px;
  margin-top: 20px;
  width: 500px;
}

</style>