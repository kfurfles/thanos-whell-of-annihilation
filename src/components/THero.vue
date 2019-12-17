<template>
    <div :class="{ first }" class="t-hero">
        <TSnapContainer ref="tsnap">
            <img class="t-hero__img" :src="require(`./../assets/images/${hero.image}`)">
        </TSnapContainer>
    </div>
</template>

<script>
import TSnapContainer from './TSnapContainer'

export default {
    props:{
        hero:{
            type: Object,
            default: () => ({ name: '', image: '' })
        }
    },
    data: () => ({
        first: false,
        disabled: false
    }),
    components:{
        TSnapContainer
    },
    methods:{
        animtate(){}
    },
    watch:{
        hero:{
            deep: true,
            handler: function(newValue){                
                this.$refs.tsnap.snap()
                this.first = true
                setTimeout(() =>{
                    this.first = false
                },4000)
            }
        }
    },
}
</script>

<style lang="scss">
    .t-hero{
        display: flex;
        align-items: center;

        &__img{
            height: 100%
        }

        &.first{
            // left: 50% !important;
            // top: 50% !important;
            // bottom: auto !important;
            // right: auto !important;
            z-index: 1;
            // transform: translate(-50%,-50%);
            // background: white;
            // border-radius: 95px;
            // box-shadow: 12px 11px 11px 1px rgba(0,0,0,0.3);
        }
    }
</style>