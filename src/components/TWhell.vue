<template>
<div>
    <canvas style="transform: rotate(180deg)" ref="canvas" width="300" height="300">
        Canvas not supported, use another browser.
    </canvas>
</div>
</template>

<script>
const rand = (min, max) => Math.random() * (max - min) + min

export default {
    props: {
        heroesData:{
            type: Array,
            default: () => []
        }
    },
    data: () => ({
        color: ['#380e7f', '#6915cf', '#d62196', '#e497cd', '#380e7f', '#6915cf', "#d62196", "#e497cd"],
        deg: rand(0, 360),
        speed: 10,
        slowDownRand: 0,
        isStopped: false,
        running: false,
    }),
    methods:{
        deg2rad(deg) {
            return deg * Math.PI / 180;
        },
        drawSlice(deg, color) {
            this.ctx.beginPath();
            this.ctx.fillStyle = color;
            this.ctx.moveTo(this.center, this.center);
            this.ctx.arc(this.center, this.center, this.width / 2, this.deg2rad(deg), this.deg2rad(deg + this.sliceDeg));
            this.ctx.lineTo(this.center, this.center);
            this.ctx.fill();
        },
        drawText(deg, text) {
            this.ctx.save();
            this.ctx.translate(this.center, this.center);
            this.ctx.rotate(this.deg2rad(deg));
            this.ctx.textAlign = "right";
            this.ctx.fillStyle = "#fff";
            this.ctx.font = 'bold 16px sans-serif';
            this.ctx.fillText(text, 130, 10);
            this.ctx.restore();
        },
        drawImg() {
            this.ctx.clearRect(0, 0, this.width, this.width);
            for (var i = 0; i < this.slices; i++) {
                this.drawSlice(this.deg, i % 2 == 0 ? '#380e7f' : '#e497cd');
                this.drawText(this.deg + this.sliceDeg / 2, this.heroes[i]);
                this.deg += this.sliceDeg;
            }
        },
        init(){
            this.deg += this.speed;
            this.deg %= 360;

            // Increment speed
            if (!this.isStopped && this.speed < 3) {
                this.speed = this.speed + 1 * 0.1;
            }
            // Decrement Speed
            if (this.isStopped) {
                if (!this.lock) {
                    this.lock = true;
                    this.slowDownRand = rand(0.984, 0.988);
                }
                this.speed = this.speed > 0.3 ? this.speed *= this.slowDownRand : 0;
            }
            // Stopped!
            if (this.lock && !this.speed) {
                var ai = Math.floor(((360 - this.deg - 90) % 360) / this.sliceDeg); // deg 2 Array Index
                ai = (this.slices + ai) % this.slices; // Fix negative index
                return this.$emit('onSelected', this.heroes[ai]) // Get Array Item from end Degree
            }

            this.drawImg();
            window.requestAnimationFrame(this.init);
        },
        run(){
            this.isStopped = false
            this.speed = 10
            this.init()
        },
        stop(){
            if(!this.isStopped){
                this.isStopped = true
            }
        }
    },
    computed:{
        ctx(){
            return this.$refs.canvas.getContext('2d')
        },
        slices(){
            return this.heroes.length
        },
        sliceDeg(){
            return 360 / this.slices
        },
        width(){
           return this.$refs.canvas.width 
        },
        center(){
            return this.width / 2
        },
        heroes(){
            return this.heroesData.map(heroes => heroes.name)
        }
    },
    watch:{
        heroesData:{
            handler: function(newValue){
                this.drawImg();
            }
        }
    },
    mounted(){
        this.drawImg();        
    }
}
</script>