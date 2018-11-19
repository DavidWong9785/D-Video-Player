<template>
  <div id="app">
    <d-video
        ref="dVideo"
        :src="src"
        :type="type"
        :rate="rate"
        :options="options"
        @on-pause="pause"
        @on-start="start"
        @on-ended="end"
        @on-error="handleError"
        @on-timeupdate="timeupdate"
        @on-loadeddata="loadeddata"
        @on-rate-change="rateChange"
        ></d-video>
  </div>
</template>

<script>
  import 'iview/dist/styles/iview.css';
  import DVideo from './components/index.js';

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
                    width: '100%',
                    height: '500px',
                    poster: 'http://www.zg-sj.com/sjphoto/photo/%E5%AD%90%E8%89%AF/201811153277494.jpg',
                },
                type: 'video/mp4',
                src: '',
                rate: 1,
                video: null,
            }
        },
        components: { DVideo },
        mounted() {
            this.rateChange(this.rate);
        },
        methods: {
            pause($event, video) {
                const vm = this;
                const timer = setTimeout(() => {
                    clearTimeout(timer);
                    vm.video.play();
                }, 3000);
            },
            start($event, video) {
                
            },
            end($event, video) {
                // console.log($event);
            },
            loadeddata($event, video) {
                this.video = video;
            },
            timeupdate(currentTime, video) {
                if (this.video && currentTime == 3) {
                    this.video.pause();
                }
            },
            handleError($event, errorMsg) {
                
            },
            rateChange(value, video) {
                // 0 超清 1 高清 2 流畅
                this.rate = value;
                switch (this.rate) {
                    case 0: 
                        this.src = 'http://component.davidwong.cn/video/123.mp4';
                        break
                    case 1: 
                        this.src = 'http://component.davidwong.cn/video/baixing.avi';
                        break;
                    case 2: 
                        this.src = 'http://component.davidwong.cn/video/hd.avi';
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
