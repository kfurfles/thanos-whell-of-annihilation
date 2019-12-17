<template>
  <div id="app">
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

export default {
    name: 'app',
    components: {
        TWhell,
        THeroesContainer,
        TButton
    },
    data: () => ({
        heroes: [
            // { name: 'B. Panter', image: 'black-panter.png', active: true },
            { name: 'C. America', image: 'captain-america.jpg', active: true },
            // { name: 'Dr. Strange', image: 'doctor-strange.png', active: true },
            // { name: 'Groot', image: 'groot.png', active: true },
            { name: 'Hulk', image: 'hulk.jpg', active: true },
            { name: 'Iron Man', image: 'iron-man.jpg', active: true },
            // { name: 'Spider Man', image: 'rocket.png', active: true },
            { name: 'Thor', image: 'thor.jpg', active: true }
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
          this.$refs.button.integrate()
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
          timer: 2000
        })
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
