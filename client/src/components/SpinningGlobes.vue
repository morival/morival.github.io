<template>
  <div class="spinning-globes" v-bind:class="{scale: hasRing}" :planet="planet">
      <div class="globe" :style="{'background-image': 'url(' + require('../assets/images/'+planet.englishName+'-map.jpg')+ ')'}"></div>
      <!-- <img v-bind:src="require(`../assets/images/${planet.englishName}-globe.jpg`)" class="earth-globe"> -->
      <div v-bind:class="{'planet-ring': hasRing}"></div>
  </div>
</template>

<script>
export default {
    name: 'spinning-globes',
    props: ['planet'],
    data() {
        return {
            hasRing: true
        }
    },
    mounted(){
        this.selectPlanet();
    },
    methods: {
        selectPlanet: function(){
            if(this.planet.englishName === 'Saturn'){
                this.hasRing = true;
            }else{
                this.hasRing = false;
            }
        }
    }

}
</script>

<style>
    .spinning-globes {
        transform-style: preserve-3d;
        transform: scale(0.8);
    }
    .scale {
        transform: scale(0.55);
    }
    .globe {
        position: relative;
        left: 50%;
        margin: auto;
        min-width: 300px;
        height: 300px;
        border-radius: 50%;
        background-size: cover;
		background-repeat: repeat-x;
        box-shadow: -20px -20px 50px 2px #000 inset, 0 0 20px 2px #000 inset;
        animation: circulate 15s linear infinite;
		transform: translate(-50%, 0) rotate(-20deg);
    }

    @keyframes circulate {
        0% {
            background-position: 0 0;
        }
        100% {
            background-position: 600px 0;
        }
    }

    .planet-ring {
	position: absolute;
    top:0;
	left: 50%;
	min-width: 660px;
	height: 660px;
/* 	border: 80px solid #918778;
	border-radius: 50%; */
	transform-origin: -27% 55%;
    transform: translate(-45%, -27px) rotate(-20deg) rotateX(80deg);
  	background: radial-gradient(ellipse at center,  rgba(24,19,25,0) 0%,rgba(53,52,51,0) 34%,rgba(55,54,52,1) 36%,rgba(56,55,53,1) 37%,rgba(68,67,66,1) 38%,rgba(56,55,53,1) 39%,rgba(68,67,66,1) 40%,rgba(56,55,53,1) 41%,rgba(87,77,76,1) 42%,rgba(87,77,76,1) 44%,rgba(113,110,103,1) 46%,rgba(113,110,103,1) 48%,rgba(113,98,93,1) 49%,rgba(113,98,93,1) 51%,rgba(122,115,105,1) 52%,rgba(113,98,93,1) 53%,rgba(113,98,93,1) 54%,rgba(122,115,105,1) 55%,rgba(106,99,89,1) 56%,rgba(106,99,89,1) 58%,rgba(79,76,76,0) 60%,rgba(65,64,70,1) 61%,rgba(65,64,70,1) 62%,rgba(90,85,89,1) 63%,rgba(78,74,73,1) 65%,rgba(78,73,74,1) 67%,rgba(78,73,74,0) 68%,rgba(78,73,75,1) 69%,rgba(78,73,75,1) 70%,rgba(78,73,76,0) 71%,rgba(77,72,76,0) 72%,rgba(24,19,25,0) 100%);
}
</style>