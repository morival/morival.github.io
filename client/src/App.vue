<template>    
  <v-app id="inspire">

  <!-- Header -->
    <v-app-bar app>
      <v-app-bar-title>
        <v-list-item  @click="reloadPage">
          <v-list-item-content>
            <v-list-item-title class="text-h6">
              Cosmodex
            </v-list-item-title>
            <v-list-item-subtitle>
              Our Solar System
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-app-bar-title>
      <v-spacer></v-spacer>
      <v-app-bar-nav-icon v-show="!drawer" @click="drawer = !drawer"></v-app-bar-nav-icon>
    </v-app-bar>

  <!-- Navigation Drower -->
    <v-navigation-drawer
    v-model="drawer"
    temporary
    right
    app>
      <v-list
      dense
      nav>
        <v-list-item
        v-for="item in drowerItems"
        :key="item.title"
        link>
          <v-list-item-content @click="show = item.link; drawer = !drawer">
            <v-list-item-title> {{item.title}} </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

  <!-- Main -->
    <v-main>
      <v-card class="d-flex flex-row">
    <!-- Planet List -->
      <planet-list 
      v-if="planets.length"
      :planets="planets"
      v-show="show === showPlanets"/>

    <!-- Planet Details -->
      <planet-detail 
      v-if="isSelected && show === showPlanets" 
      :moons="moons" 
      :planets="planets" 
      :planet="isSelected" 
      :getMoons="getMoons()" 
      :descriptions="descriptions"
      :milesConvertor="({props}) => Math.round(props/ 1.609)"
      v-show="show === showPlanets"/>

    <!-- Moon Detail -->
      <moon-detail
      v-if="showMoon && show === showPlanets"
      :moon="selectedMoon" 
      :isSelected="isSelected" 
      :planets="planets" 
      :planet="isSelected"
      :milesConvertor="({props}) => Math.round(props/ 1.609)"/>

    <!-- Animation -->
      <div class="planet-animation">
        <planet-animation 
        :planets="planets" 
        v-show="show === showAnimation"/>
      </div>

    <!-- Grossary -->
      <div class="glossary">
        <glossary 
        :descriptions="descriptions" 
        v-show="show === showGlossary"/>
      </div>
      </v-card>
    </v-main>

  <!-- Footer -->
    <v-footer app>
    <!-- -->
    </v-footer>
  </v-app>
</template>

<script>

import PlanetAnimation from './components/PlanetAnimation.vue'
// import ListedPlanet from './components/ListedPlanet.vue'
import PlanetList from './components/PlanetList.vue'
import PlanetDetail from './components/PlanetDetail.vue'
import MoonList from './components/MoonList.vue'
import HubbleServices from './services/HubbleServices.js'
import MoonDetails from '@/components/MoonDetails';
import Glossary from '@/components/Glossary.vue';
import { eventBus } from '@/main.js';
import axios from 'axios';


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
      drawer: null,
      drowerItems: [
        { title: 'Planets', link: 1 },
        { title: 'Animation', link: 2},
        { title: 'Glossary', link: 3}
      ],
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

    // Activates description of selected planet
    eventBus.$on('planet-selected', planet => {
      if (this.isSelected !== planet) {
        this.isSelected = planet;
        this.showMoon = false
      } else {
        this.isSelected = null;
      }
    });
    // Activates description of selected moon
    eventBus.$on('moon-selected', moon => {
      this.selectedMoon = moon;
      this.showMoon = true;
      this.isSelected = null;
    });
    // Returns to mother planet of the previously selected moon
    eventBus.$on('return-planet', orbitsPlanet => {
      this.isSelected = this.planets.find(planet => planet.rel === orbitsPlanet);
      this.showMoon = false;
    });
  },

  methods: {
    // Gets all bodies (planets and moons) and set them in the right data arrays
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
          }
        }
      });
    },

    // Filters through moon list for the planet?
    getMoons: function(){
      if (this.isSelected.moons) {
      let planetsRel = this.isSelected.moons.map(planetsMoon => planetsMoon.rel); 
      return this.moons.filter(function(moon) {
        return planetsRel.indexOf(moon.rel) != -1;
        });
      }
    },

    // Gets all descriptions from MongoDB
    getDescriptions: function(){
      HubbleServices.getDescriptions()
      .then(data => this.descriptions = data)
    },
    reloadPage() {
      window.location.reload();
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
  background-color: black;
  /* background-image: url('../src/assets/images/d099fbe1334992232264f479a516983e.jpg'); */
  /* background-repeat:inherit; */
  background-size: 100%;
  background-attachment: fixed;
  display: flex;
  flex-direction: column;
  align-content: center;
}

.header {
  display: flex;
  margin-top: 30px;
  justify-content: space-between;
  z-index: 3;
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
      /* font-size: 60px; */
    }
    #logo-h4 {
      /* font-size: 16px; */
    }
}
  
</style>

