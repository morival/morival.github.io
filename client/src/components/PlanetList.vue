<template>
    <!-- Filter Drawer -->
    <v-navigation-drawer permanent>
    <!-- <div class="view-cosmodex"> -->
        <v-menu>
            <template v-slot:activator="{ on, attrs }">
                <v-btn
                color="primary"
                dark
                v-bind="attrs"
                v-on="on">
                    Filter by: {{ isActive }}
                </v-btn>
            </template>
            <v-list>
                <v-list-item
                v-for="(filterType, index) in filterTypes"
                :key="index"
                link>
                    <v-list-item-content @click="filterType.compare">
                        <v-list-item-title>{{ filterType.title }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-menu>
        <!-- <div v-show="filter === false" class="filter-buttons">
            <button @click="showFilters" id="filter-btn" class="btn"> Filter by: {{ isActive }} <span/></button>
        </div>
        <div v-show="filter === true" class="filter-buttons">
            <button @click="compareDistance" class="btn"> distance from the Sun <span/></button>
            <button @click="compareSize" class="btn"> size <span/></button>
            <button @click="compareDensity" class="btn"> density <span/></button>
            <button @click="compareGravity" class="btn"> gravity <span/></button>
        </div> -->
        <v-divider></v-divider>
        <!-- <div class="sun-image" v-bind:class="{'sun-transition': showSun}" :style="{'background-image': `url(${require('../assets/images/sun_slice.png')})`}"/> -->
    <!-- List of Planets -->
        <v-list class="d-flex flex-column align-center">
            <listed-planet 
            v-for="(planet, index) in filterPlanets" 
            :planet="planet" 
            :key="index" 
            :isActive="isActive"/>
        </v-list>
        <!-- <div class="planet-list" v-if="planets.length">
            
        </div> -->
    <!-- </div> -->
    </v-navigation-drawer>
</template>

<script>
// import {eventBus} from '@/main.js';
import ListedPlanet from '@/components/ListedPlanet.vue';
// import sunImage from '../assets/images/sun_slice.png';

export default {
    name: 'planet-list',
    props: ['planets'],
    components: {
        'listed-planet': ListedPlanet
    },
    data() {
        return {
            filter: false,
            filterTypes: [
                { title: 'distance from the Sun', compare: this.compareDistance },
                { title: 'size', compare: this.compareSize},
                { title: 'density', compare: this.compareDensity},
                { title: 'gravity', compare: this.compareGravity}
            ],
            filterPlanets: null,
            isActive: "",
            showSun: false,
            // sunImage: sunImage
        }
    },
    mounted(){
        this.compareDistance();
    },
    computed: {
        sortByDistance: function(){
            return this.planets.sort((a, b) => a.semimajorAxis - b.semimajorAxis);
        },
        sortBySize: function(){
            return this.planets.sort((a, b) => a.meanRadius - b.meanRadius);
        },
        sortByDensity: function(){
            return this.planets.sort((a, b) => a.density - b.density);
        },
        sortByGravity: function(){
            return this.planets.sort((a, b) => a.gravity - b.gravity);
        }
    },
    methods: {
        showFilters: function(){
            this.filter = true;
        },
        compareDistance: function(){
            this.filter = false;
            this.filterPlanets = this.sortByDistance;
            this.isActive = "distance";
            this.showSun = true;
        },
        compareSize: function(){
            this.filter = false;
            this.filterPlanets = this.sortBySize;
            this.isActive = "size";
            this.showSun = false;
        },
        compareDensity: function(){
            this.filter = false;
            this.filterPlanets = this.sortByDensity;
            this.isActive = "density";
            this.showSun = false;
        },
        compareGravity: function(){
            this.filter = false;
            this.filterPlanets = this.sortByGravity;
            this.isActive = "gravity";
            this.showSun = false;
        }
    }
};
</script>

<style>

.view-cosmodex {
    display: flex;
    flex-direction: column;
    width: 100%;
}

.filter-buttons {
    right: 70px;
    display: flex;
    z-index: 3;
    /* margin: 0 20px 20px 0; */
}
.sun-image {
    position: fixed;
    width: 373px;
    height: 1258px;
    /* z-index: 2; */
    margin-top: -450px;
    margin-left: -500px;
    transition: all 1s ease-out;
    overflow: hidden;
}
.sun-transition {
    transform:translateX(220px);
}

.planet-list {
  margin: 15px 0;
  /* margin-left: 5%; */
  border: 0;
  padding: 0;
  width: 90%;
  height: 140px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;  
  align-items: center;
  z-index: 3;
}

@media only screen and (max-width: 1024px) {
    /* .view-cosmodex {
    } */
    .planet-list, .filter-buttons {
        flex-direction: column;
    }
    .planet-list {
        position: absolute;
        top: 220px;
        align-items: flex-start;
        justify-content: flex-start;
        height: auto;
        width: 110px;
        z-index: 1;
    }
    .filter-buttons {
        align-items: flex-end;
        position: fixed;
        top: 210px;
        right: 5%;
        background-color: black;
        /* z-index: 2; */
    }
    .sun-image {
        /* background-image: url(/img/sun_slice.52a37a0e.png); */
        /* make the Sun's glow transparent */
        mask-image: linear-gradient(to left, transparent 0%, black 25%);
        margin-left: -200px;
        margin-top: -850px;
        transform: rotate(90deg);
        z-index: 2;
    }
    .sun-transition {
        transform: rotate(90deg) translateX(250px);
    }
}
</style>
