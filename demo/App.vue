<template>
    <div id="app">
        <d-video
            :src="src"
            :type="type"
            :rate="rate"
            :options="options"
            @on-pause="pause"
            @on-start="start"
            @on-ended="end"
            @on-loadeddata="loadeddata"
            @on-rate-change="rateChange"
            ></d-video>
    </div>
</template>

<script>
    import DVideo from 'd-video-player';
    export default {
        name: 'App',
        data() {
            return {
                options: {
                    autoplay: false,
                    muted: false,
                    loop: false,
                    noRatePanel: false,
                    preload: 'auto',
                    poster: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1539515544505&di=7062a1a16173658a2bfc9b07abe1578a&imgtype=0&src=http%3A%2F%2Fis2.mzstatic.com%2Fimage%2Fthumb%2FPurple4%2Fv4%2F0d%2Fb6%2F6c%2F0db66c10-4b45-f145-dcae-94fa8022c95e%2Fsource%2F512x512bb.jpg',
                },
                type: 'video/mp4',
                src: '',
                rate: 1,
            }
        },
        components: { DVideo },
        mounted() {
            this.rateChange(this.rate);
        },
        methods: {
            pause($event, video) {
                // console.log('pause');
                // console.log($event);
            },
            start($event, video) {
                // console.log('start')
                // const vm = this;
                // setTimeout(() => {
                //     video.pause();
                // }, 2000);
                // console.log(this.$refs.dVideo.$refs.controlNav.togglePlay);
                // console.log($event);
            },
            end($event, video) {
                // console.log($event);
            },
            loadeddata($event, video) {

            },
            timeupdate($event, video) {

            },
            rateChange(value, video) {
                // 0 超清 1 高清 2 流畅
                this.rate = value;
                switch (this.rate) {
                    case 0: 
                        this.src = 'http://localhost:8092/414f5010b60df3b5a0a2294fc492d125/f4b35bfcc18c37f13f9c7ce87d6b2e4d/prepare.avi';
                        break
                    case 1: 
                        this.src = 'http://localhost:8092/414f5010b60df3b5a0a2294fc492d125/f4b35bfcc18c37f13f9c7ce87d6b2e4d/prepare1.mp4';
                        break;
                    case 2: 
                        this.src = 'http://localhost:8092/414f5010b60df3b5a0a2294fc492d125/f4b35bfcc18c37f13f9c7ce87d6b2e4d/prepare2.mp4';
                        break;
                }
            }
        }
    }

</script>

<style>
    #app {
        width: 1000px;
        margin: 0 auto;
    }

</style>
