<template>
    <div ref="content" class="content">
        <slot></slot>
    </div> 
</template>

<script>
import html2canvas from 'html2canvas'
import anime from 'animejs'
import change from 'chance'

import { onMounted } from '@vue/composition-api'

export default {
    props:{
        canvasCount:{
            type: Number,
            default: () => 60
        }
    },
    setup(props, context) {
        const imageDataArray = [];
        const canvasCount = props.canvasCount;
        

        function snap(){
            const content = context.refs.content
            const getNotDust = (cb) => content.querySelectorAll(":not(.dust)").forEach(nodeEl =>{
            cb(nodeEl)
            })

            html2canvas(content).then(canvas => {
                canvas.setAttribute('style', `width: ${canvas.width}px !important; height: ${canvas.height}px !important`)
                const ctx = canvas.getContext("2d");
                const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
                const pixelArr = imageData.data;
                createBlankImageData(imageData);
                //put pixel info to imageDataArray (Weighted Distributed)
                for (let i = 0; i < pixelArr.length; i += 4) {
                    //find the highest probability canvas the pixel should be in
                    let p = Math.floor((i / pixelArr.length) * canvasCount);
                    let a = imageDataArray[weightedRandomDistrib(p)];
                    a[i] = pixelArr[i];
                    a[i + 1] = pixelArr[i + 1];
                    a[i + 2] = pixelArr[i + 2];
                    a[i + 3] = pixelArr[i + 3];
                }
                //create canvas for each imageData and append to target element
                for (let i = 0; i < canvasCount; i++) {
                    let c = newCanvasFromImageData(imageDataArray[i], canvas.width, canvas.height);
                    c.classList.add("dust");
                    content.append(c);
                }
                //clear all children except the canvas
                getNotDust(nodeEl => nodeEl.classList.add("fadeOut"))
                //apply animation
                document.querySelectorAll(".dust").forEach((canvas, index) => {
                    animateBlur(canvas, 0.8, 800);
                    setTimeout(() => {
                        animateTransform(canvas, 100, -100, chance.integer({
                            min: -15,
                            max: 15
                        }), 800 + (110 * index));
                    }, 70 * index);
                    //remove the canvas from DOM tree when faded
                    removeOnFaded(canvas, 70 * index, (110 * index) + 800)
                });
            });
        }

        function weightedRandomDistrib(peak) {
            const prob = [],
                seq = [];
            for (let i = 0; i < canvasCount; i++) {
                prob.push(Math.pow(canvasCount - Math.abs(peak - i), 3));
                seq.push(i);
            }
            return chance.weighted(seq, prob);
        }

        function removeOnFaded(elem, delay, duration) {
            anime({
                targets: elem,
                delay: delay,
                opacity: 0,
                easing: "easeInOutSine",
                duration: duration,
                complete: () => {
                    elem.remove()
                }
            })
        }

        function animateBlur(elem, radius, duration) {
            anime({
                targets: elem,
                duration: duration,
                filter: ['blur(' + 0 + 'px)', 'blur(' + radius + 'px)'],
                easing: "easeOutQuad",
            })
        }

        function animateTransform(elem, sx, sy, angle, duration) {
            anime({
                targets: elem,
                duration: duration,
                rotate: angle,
                translateX: sx,
                translateY: sy,
                easing: "easeOutQuad",
            })
        }

        function createBlankImageData(imageData) {
            for (let i = 0; i < canvasCount; i++) {
                let arr = new Uint8ClampedArray(imageData.data);
                for (let j = 0; j < arr.length; j++) {
                    arr[j] = 0;
                }
                imageDataArray.push(arr);
            }
        }

        function newCanvasFromImageData(imageDataArray, w, h) {
            const canvas = document.createElement('canvas');
            canvas.width = w;
            canvas.height = h;
            const tempCtx = canvas.getContext("2d");
            tempCtx.putImageData(new ImageData(imageDataArray, w, h), 0, 0);

            return canvas;
        }

        return { 
            snap
        }

    }
}
</script>

<style lang="scss">
.content{
    position: relative;
    width: 100%;
    height: 100%;
      
    canvas{
        width: 100%;
        height: 100%;
        position: absolute;
        left: 0;
        top: 0;
    }
}

@keyframes fadeOut {
    0% {opacity: 1;}
    100% {opacity: 0;}
}

.fadeOut {
    animation: fadeOut linear 3500ms forwards;
}
</style>