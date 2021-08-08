<template>
  <div id="app">
    <!-- header/logo starts here -->
    <div class=header>
      <div class=logo-container>
        <h1 class="logo-text" id="logo-h1">Cosmodex</h1>
        <h4 class="logo-text" id="logo-h4"> Solar System By <span class="brackets">{{</span>The Curly Boys<span class="brackets">}}</span></h4>
      </div>

      <!-- main buttons here -->
      <div class="main-menu">
        <button class="btn" @click="show = showPlanets"> Planets <span/></button>
        <button class="btn" @click="show = showAnimation; showMoon=false"> Solar System In Action <span/></button>
        <button class="btn" @click="show = showGlossary; showMoon=false"> What Does That Mean?! <span/></button>
      </div>
    </div>

    <!-- // list of planets starts here -->
      <planet-list 
        v-if="planets.length"
        :planets="planets"
        v-show="show === showPlanets"/>

    <!-- planet details starts here -->
      <planet-detail 
        v-if="isSelected && show === showPlanets" 
        :moons="moons" 
        :planets="planets" 
        :planet="isSelected" 
        :getMoons="getMoons()" 
        :descriptions="descriptions"
        :milesConvertor="({props}) => Math.round(props/ 1.609)"
        v-show="show === showPlanets"/>

    <!-- moon details starts here -->
      <moon-detail
        v-if="showMoon"
        :moon="selectedMoon" 
        :isSelected="isSelected" 
        :planets="planets" 
        :planet="isSelected"
        :milesConvertor="({props}) => Math.round(props/ 1.609)"/>

    <!-- animation starts here -->
    <div class="planet-animation">
      <planet-animation 
        :planets="planets" 
        v-show="show === showAnimation"/>
    </div>

    <!-- grossary starts here -->
    <div class="glossary">
      <glossary 
        :descriptions="descriptions" 
        v-show="show === showGlossary"/>
    </div>
  </div>
</template>

<script>

import PlanetAnimation from './components/PlanetAnimation.vue'
import ListedPlanet from './components/ListedPlanet.vue'
import PlanetList from './components/PlanetList.vue'
import PlanetDetail from './components/PlanetDetail.vue'
import MoonList from './components/MoonList.vue'
// import HubbleServices from './services/HubbleServices.js'
import MoonDetails from '@/components/MoonDetails';
import Glossary from '@/components/Glossary.vue';
import { eventBus } from '@/main.js';
import axios from 'axios'
// import VueAxios from 'vue-axios'


export default {
  name: 'App',
  components: {
    'planet-list': PlanetList,
    'moon-list': MoonList,
    'planet-detail': PlanetDetail,
    'planet-animation': PlanetAnimation,
    'moon-detail': MoonDetails,
    'glossary' : Glossary,
  },
  data(){
    return {
      planets: [],
      moons: [],
      isSelected: null,
      selectedMoon: null,
      descriptions: [],
      show: null,
      showPlanets: 1,
      showAnimation: 2,
      showGlossary: 3,
      showMoon: false
    }
  },
  mounted(){
    this.getPlanets();
    this.getDescriptions();
    eventBus.$on('planet-selected', planet => {
      if (this.isSelected !== planet) {
        this.isSelected = planet;
        this.showMoon = false
      } else {
        this.isSelected = null;
      }
    });

    eventBus.$on('moon-selected', moon => {
      this.selectedMoon = moon;
      this.showMoon = true;
      this.isSelected = null;
      });
    
    eventBus.$on('return-planet', orbitsPlanet => {
      this.isSelected = this.planets.find(planet => planet.rel === orbitsPlanet);
      this.showMoon = false;
      
    });
  },

  methods: {
    getPlanets: function(){
      let planetTemp = []
      fetch('https://api.le-systeme-solaire.net/rest/bodies/')
      .then(res => res.json())
      .then(planets => planetTemp = planets.bodies)
      .then(() => {
        if(planetTemp.length) {
          for (let body of planetTemp){
            if (body.isPlanet && body.gravity >=1) {
              this.planets.push(body)
            }
            else if (!body.isPlanet) {
              this.moons.push(body);
          }
        }}
      });
    },

    // Filters through moon list for the planet?
    getMoons: function(){
      if (this.isSelected.moons) {
      let planetsRel = this.isSelected.moons.map(planetsMoon => planetsMoon.rel); 
      return this.moons.filter(function(moon) {
        return planetsRel.indexOf(moon.rel) != -1;
        });
        // console.log(this.description);
      }
    },
    // getDescriptions: function(){
    //   HubbleServices.getDescriptions()
    //   .then(data => this.descriptions = data)
    // },
    
    async getDescriptions(){
      axios.defaults.headers.common['Access-Control-Allow-Origin'] = '*';
      const url = 'https://hubblesite.org/api/v3/glossary';
      const headers = { 
        // 'Access-Control-Allow-Credentials': 'true',
        // 'Access-Control-Allow-Headers': "Origin, Content-Type, X-Auth-Token",
        // 'Content-Type': 'text/html; charset=utf-8',
        // 'Access-Control-Allow-Origin': '*',
        // 'Origins': '*'
        // 'Access-Control-Allow-Methods' : 'GET'
        };
      // const response = await axios.get(url);
      // const results = response.data.results;
      // console.log(response);
      
      const response = axios.get(url, {
        headers
        // mode:'cors',
        // headers: {'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Headers': "*"}
        })
        .then(res => this.descriptions = res.data)
        .then(console.log(this.descriptions))
    // },
    // milesConvertor: function(number){
    //   return Math.round(number/ 1.609)
    }
    
  }   
}
</script>

<style>

html {
  height: 100%;
}

body {
  font-family: Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  margin: 0;
  border: 0;
  padding: 0;
  /* height: 940px; */
  overflow: scroll;
  background-color: black;
  /* background-image: url('../src/assets/images/d099fbe1334992232264f479a516983e.jpg'); */
  /* background-repeat:inherit; */
  background-size: 100%;
  background-attachment: fixed;
  display: flex;
  flex-direction: column;
  align-content: center;
  /* z-index: -2; */
}

.header {
  display: flex;
  margin-top: 30px;
  justify-content: space-between;
}

.logo-text {
  color: black;
  text-shadow: 0 2px 10px rgb(0, 89, 255), 0 2px 30px rgba(255, 255, 255, 0.733);
  position: relative;
  font-family: 'Gugi', cursive;
  margin: 0;
}
#logo-h1 {
  align-content: center;
  font-size: 30px;
}

#logo-h4 {
  /* margin: 5px 10px; */
  position: relative;
  font-size: 15px;
}

.brackets {
  color: #940000;
}

#app {
  margin: 0;
  padding: 0;
  border: 0;
  width:90%;
  height: 100%;
  margin:auto;
}


.main-menu {
  display: flex;
  /* flex-direction: column; */
  justify-content: flex-end;
}

.btn {
  width: 200px;
  padding: 5px;
  padding-right: 20px;
  margin: 10px;
  text-decoration: none;
  border: none;  
  border-radius: 4px;
  transition-duration: 0.1s;
  cursor: pointer;
  color: white;
  background-image: linear-gradient(60deg, black, darkblue, blue, grey, white);
  
  
}

.btn:hover {
  color: crimson;
  cursor: pointer;
  /* background-color: black; */
}

.btn:active {
  color: crimson;
  border: none;
  /* background-color: black; */
}

/* .btn span {
  color: rgb(192, 17, 52);
  width: auto;
  border: none;
  cursor: pointer;
  display: inline;
  position: relative;
  transition: 0.1s;
} */

.btn span:after {
  color: rgb(192, 17, 52);
  width: auto;
  border: none;
  /* content: "Click Me"; */
  text-shadow: 5px 2px 20px rgba(0, 26, 73, 0.404);
  display: inline;
  position: relative;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.1s;
}

.btn:hover span {
  padding-right: 15px;
}

.btn:hover span:after {
  opacity: 1;
  right: 0;
}

.khaki {
    color:khaki;
}
.red {
    color: red;
}

@media only screen and (max-width: 1024px) {
    .header {
      /* position: sticky;
      top: 30px; */
      position:fixed;
      width: 90%;
      z-index: 3;
    }
    #logo-h1 {
      font-size: 60px;
    }
    #logo-h4 {
      font-size: 16px;
    }
}
  
</style>
