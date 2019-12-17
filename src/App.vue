<template>
  <div id="app">
      <TFrame :video="endVideo" ref="video"/>
      <div class="thanos">
          <TWhell class="whell" ref="wheel" :heroesData="availableHeros" @onSelected="selectedHero" />
          <img class="manopla" src="./assets/manopla.png" alt="" srcset="">
      </div>
      <TButton class="desintegrate-button" ref="button" @onClick="runWhell" disabled @click="() => ''"/>
      <div class="avengers">
        <THeroesContainer :heroes="heroes" />
      </div>
  </div>
</template>

<script>

import './plugins/particules'
import Swal from 'sweetalert2'

import TWhell from './components/TWhell.vue'
import TButton from './components/TButton.vue'
import THeroesContainer from './components/THeroesContainer.vue'
import TFrame from './components/TFrame.vue'

export default {
    name: 'app',
    components: {
        TWhell,
        THeroesContainer,
        TButton,
        TFrame
    },
    data: () => ({
        endVideo: '',
        heroes: [
            { name: 'C. America', image: 'captain-america.jpg', active: true, movie: 'https://www.youtube.com/watch?v=eKegchGOcXc' },
            { name: 'Hulk', image: 'hulk.jpg', active: true, movie: 'https://www.youtube-nocookie.com/embed/kO5WPKT0690' },
            { name: 'Iron Man', image: 'iron-man.jpg', active: true, movie: 'https://www.youtube-nocookie.com/embed/GUMGjwgAA9M' },
            { name: 'Thor', image: 'thor.jpg', active: true, movie: 'https://www.youtube-nocookie.com/embed/X6fWfoax5bU' }
        ]
    }),
    methods:{
      runWhell(){
        this.$refs.button.disintegrate()
        this.$refs.wheel.run()
        setTimeout(() =>{
          this.$refs.wheel.stop()
        }, 2500)
      },
      selectedHero(hero){
        setTimeout(() =>{
          if(this.availableHeros.length > 1){
            this.$refs.button.integrate()
          }
        },5000)
        this.heroes = this.heroes
        const idx = this.heroes.findIndex(h => h.name === hero)
        this.heroes[idx].active = false
        this.bye(this.heroes[idx].name)
        this.heroes = [...this.heroes]
      },
      bye(name){
        Swal.fire({
          title: `snap ...`,
          text: `bye, ${name}`,
          imageUrl: require('./assets/snap.gif'),
          imageWidth: 400,
          imageHeight: 200,
          imageAlt: 'Custom image',
          showCancelButton: false,
          showConfirmButton: false,
          timer: 2000,
          allowOutsideClick: false
        })
      }
    },
    watch:{
      availableHeros(heroes){
        if (heroes.length === 1) {
          setTimeout(() =>{
            this.endVideo = heroes[0].movie
            this.$refs.video.open()
          },8000)
        }
      }

    },
    computed:{
      availableHeros(){
        return this.heroes.filter(hero => hero.active)
      },
    }
}
</script>

<style lang="scss">

@import './base';

#app {
  text-align: center;
  color: #2c3e50;
  min-height: 100vh;
  display: flex;
  min-width: 100%;
  overflow: hidden;
}

.thanos{
  width: 30%;
  position: relative;
  color: #2c3e50;
  min-height: 100vh;
  background: url('./assets/thanos-smile.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;

  .manopla{
    width: 155px;
    height: 247px;
    position: absolute;
    right: 0;
    bottom: 0;
    transform: translateX(50%);
  }

  .whell{
    position: absolute;
    bottom: 139px;
    right: -80px;
    padding: 5rem;
    transform: translateX(calc(50% - 80px));
  }
}

.desintegrate-button{
  position: absolute !important;
  top: -5rem;
  left: calc(30% - 7rem);
  transform: translateX(calc(-50% + 7rem));
}

.avengers{
  padding-left: 150px;
  width: 70%;
  height: 100vh;
  position: relative;
  background: #fff;
  z-index: -1;
}

</style>
