<template>
<!-- https://www.youtube-nocookie.com/embed/eKegchGOcXc -->
    <div class="t-frame" :class="{ open: showVideo }">
        <iframe v-if="showVideo" width="950" height="515" ref="video" :src="video" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        <button @click="reset()">Star Over</button>
    </div>
</template>

<script>
import '../plugins/particules'
import { onMounted } from '@vue/composition-api'

export default {
    props: {
        video:{
            type: String
        }
    },
    data: () => ({
        showVideo: false
    }),
    methods:{
        open(){
            console.log(this.$refs.video)
            this.showVideo = true
        },
        reset(){
            window.location.reload()
        }
    }
}
</script>

<style lang="scss">
.t-frame{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-flow: column;
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 7;
    background: rgba(0,0,0,0.9);
    opacity: 0;
    visibility: hidden;


    &.open{
        opacity: 1;
        visibility: visible
    }

    button{
        padding: 8px 21px;
        background: #fff;
        border: none;
        border-radius: 5px;
        font-size: 22px;
        margin-top: 40px;
    }
}
</style>