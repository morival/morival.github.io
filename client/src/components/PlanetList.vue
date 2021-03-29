<template>
    <!-- this should consider the format of each item to be displayed -->
    <div class="view-cosmodex">
        
        <div class="filter-buttons">
            <button v-on:click="compareDistance" class="main-button"> Filter By Distance from the Sun <span> </span></button>
            <button v-on:click="compareSize" class="main-button"> Filter Planets by Size <span> </span></button>
            <button v-on:click="compareDensity" class="main-button"> Filter Planets by Density <span> </span></button>
            <button v-on:click="compareGravity" class="main-button"> Filter Planets by Gravity <span> </span></button>
        </div>
        <img id="sun-image" src="../assets/images/Sun.png">
        <div class="planet-list" v-if="planets.length">
        
            <listed-planet v-for="(planet, index) in filterPlanets" :planet="planet" :key="index" :isActive="isActive" :showShadow="showShadow" />
        </div>
    
    </div>
    
</template>

<script>
import {eventBus} from '@/main.js';
import ListedPlanet from '@/components/ListedPlanet.vue';

export default {
    name: 'planet-list',
    props: ['planets'],
    components: {
        'listed-planet': ListedPlanet
    },
    data() {
        return {
            filterPlanets: null,
            isActive: "",
            showShadow: ""
        }
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
        compareDistance: function(){
            this.filterPlanets = this.sortByDistance;
            this.isActive = "distance";
            this.showShadow = "";
            // eventBus.$emit('planet-by-size', isActive);
        },
        compareSize: function(){
            this.filterPlanets = this.sortBySize;
            this.isActive = "size";
            this.showShadow = "showShadow";
            // eventBus.$emit('planet-by-size', isActive);
        },
        compareDensity: function(){
            this.filterPlanets = this.sortByDensity;
            this.isActive = "density";
            this.showShadow = "";
        },
        compareGravity: function(){
            this.filterPlanets = this.sortByGravity;
            this.isActive = "gravity";
            this.showShadow = "";
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
    position: absolute;
    z-index: -1;
    margin-top: -750px;
    margin-left: -1350px;
}
</style>
