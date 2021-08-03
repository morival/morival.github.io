<template>
  <div>
  <div class="selected-moon">
    <div>
      <button @click="returnPlanet(moon)" class="btn">Back to {{ showAroundPlanet(moon) }}</button>
      <h2 v-if="moonName(moon) != 'Moon'">{{ moonName(moon) }} Moon</h2>
      <h2 v-if="moonName(moon) == 'Moon'">{{ moonName(moon) }}</h2>
      <img v-bind:src="require(`../assets/images/Moon.png`)" title="picture" alt="picture of chosen planet" height="300px" />
    </div>
    <div class="moon-container">
      <!-- <div class="moon-label">
      </div> -->
      <div class="listed-moon-details">
        <p>Orbits: {{ showAroundPlanet(moon) }}</p>
        <div v-if="moon.semimajorAxis != 0">
          <p @click="convertDistance = !convertDistance" v-show="convertDistance">Average Distance from Sun: {{moon.semimajorAxis}} <span class="metric">km</span></p>
          <p @click="convertDistance = !convertDistance" v-show="!convertDistance">Average Distance from Sun: {{milesConvertor(moon.semimajorAxis)}} <span class="metric">miles</span></p>
        </div>
        <div v-else> 
          <p>Average Distance from Sun Not Available</p>
        </div>
        <div v-if="moon.sideralOrbit != 0">
          <p>Time to Orbit the Planet: {{moon.sideralOrbit}} days </p>
        </div>
        <div v-else> 
          <p>Time to Orbit the Planet Not Available</p>
        </div>
        <div v-if="moon.sideralRotation != 0">
          <p>Time to Spin on Axis (a day): {{Math.round(moon.sideralRotation)}} hours </p>
        </div>
        <div v-else> 
          <p>Time to Spin on Axis Not Available</p>
        </div>
        <div v-if="moon.equaRadius != 0">
          <p @click="convertDistance = !convertDistance" v-show="convertDistance">Average Radius: {{Math.round(moon.equaRadius)}} <span class="metric">km</span></p>
          <p @click="convertDistance = !convertDistance" v-show="!convertDistance">Average Radius: {{milesConvertor(moon.equaRadius)}} <span class="metric">miles</span></p>
        </div>
        <div v-else> 
          <p>Average Radius Not Available</p>
        </div>
        <div v-if="moon.gravity != 0">
          <p>Gravity: {{ moon.gravity }}m/s² </p>
        </div>
        <div v-else> 
          <p>Gravity Not Available</p>
        </div>
      </div>
    </div>
  </div>
  </div>
  
</template>

<script>
  import {eventBus} from '@/main';

  export default {
    name: 'moon-detail',
    props: ['moon', 'isSelected', 'planets'],
    data() {
        return {
            convertDistance: true
        }
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
      },
      milesConvertor: function(number){
            return Math.round(number/ 1.609)
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