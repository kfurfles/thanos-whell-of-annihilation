<template>
    <div :class="[ 'hero-container', remainClass]" >
        <THero class="hero" v-for="hero of heroes" :key="hero.name" :hero="hero" />
    </div>
</template>

<script>
    import THero from './THero'

    export default {
        props:{
            heroes:{
                type: Array
            }
        },
        components:{
            THero
        },
        computed:{
            remainClass(){
                return `remain-${this.heroes.filter(e => e.active).length}`
            }
        }
    }
</script>

<style lang="scss">
.hero-container{
    position: relative;
    width: 100%;
    height: inherit;

    .hero{
        position: absolute;
        width: 50%;
        height: 50%;
        transition: cubic-bezier(0.075, 0.82, 0.165, 1) 500ms all;

        &:nth-child(1){
            left: 0;
            top: 0;
        }
        &:nth-child(2){
            left: 0;
            bottom: 0;
        }
        &:nth-child(3){
            right: 0;
            top: 0;
        }
        &:nth-child(4){
            right: 0;
            bottom: 0;
        }
    }
}
</style>