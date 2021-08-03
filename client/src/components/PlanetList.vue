<template>
    <!-- this should consider the format of each item to be displayed -->
    <div class="view-cosmodex">
        <div v-show="filter === false" class="filter-buttons">
            <button @click="showFilters" id="filter-btn" class="main-button"> Filter by: {{ isActive }} <span/></button>
        </div>
        <div v-show="filter === true" class="filter-buttons">
            <button @click="compareDistance" class="main-button"> distance from the Sun <span/></button>
            <button @click="compareSize" class="main-button"> size <span/></button>
            <button @click="compareDensity" class="main-button"> density <span/></button>
            <button @click="compareGravity" class="main-button">  gravity <span/></button>
        </div>

        <img id="sun-image" v-bind:class="{'sun-transition': showSun}" src="../assets/images/sun_slice.png">
        <div class="planet-list" v-if="planets.length">
            <listed-planet 
            v-for="(planet, index) in filterPlanets" :planet="planet" :key="index" 
            :isActive="isActive"/>
        </div>
    
    </div>
    
</template>

<script>
// import {eventBus} from '@/main.js';
import ListedPlanet from '@/components/ListedPlanet.vue';

export default {
    name: 'planet-list',
    props: ['planets'],
    components: {
        'listed-planet': ListedPlanet
    },
    data() {
        return {
            filter: false,
            filterPlanets: null,
            isActive: "",
            showSun: false
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
    position:relative;
    right: 70px;
    display: flex;
    align-items: right;
    margin: 0 20px 20px 0;
    /* z-index: 1; */
}
#sun-image {
    position: fixed;
    z-index: -1;
    margin-top: -450px;
    margin-left: -500px;
    transition: all 1s ease-out;
    overflow: hidden;
}
.sun-transition {
    transform:translateX(220px);
}
</style>
