<!--  -->
<template>
    <div class="d-video-box" @mousemove="controllBottomNav" @dblclick="dbclickToggleScreen">
        <div ref="videoBox" class="d-video-panel">
            <video
                @click="pause" 
                ref="video" 
                style="width: 100%;">
                <source :src="src" :type="type">
            </video>
            <div class="big-play-logo" v-if="!playStatus && !errorTag" @click="play">
                <Icon type="md-play" />
            </div>
            <div class="error-msg" v-if="errorTag">
                <span>出错了，该视频无法播放</span>
            </div>
        </div>
        <transition name="fade">
            <div class="controlNav" v-show="show" ref="controlNav">
                <!-- 播放/暂停 -->
                <play-pause 
                    :class="this.noRatePanel? 'noRatePanel' : ''" 
                    :playStatus="playStatus"
                    @on-play-status-change="togglePlay"></play-pause>
                <!-- 时间 -->
                <show-times 
                    :class="this.noRatePanel? 'noRatePanel' : ''"
                    :currentTime="currentTime"
                    :duration="duration"></show-times>
                <!-- 进度条 -->
                <progress-bar 
                    :class="this.noRatePanel? 'noRatePanel' : ''" 
                    :value="progress"
                    @on-progress-bar-change="progressBarChange"></progress-bar>
                <!-- 音量控制 -->
                <volumePanel 
                    :class="this.noRatePanel? 'noRatePanel' : ''" 
                    :value="volumeProgress" 
                    :volumeStatus="volumeStatus"
                    @on-progress-bar-change="volumeChange" 
                    @on-toggle-volume="toggleVolume"></volumePanel>
                <!-- 码率选择 -->
                <ratePanel 
                    :rate="rate"
                    v-if="!this.noRatePanel"
                    @on-rate-change="rateChange"></ratePanel>
                <!-- 全屏 -->
                <full-screen 
                    :class="this.noRatePanel? 'noRatePanel' : ''" 
                    :screenStatus="screenStatus"
                    @on-full-cscreen-change="toggleScreen"></full-screen>
            </div>
        </transition>
    </div>

</template>

<script>
    import playPause from './component/play-pause/play-pause.vue'
    import showTimes from './component/show-times/show-times.vue'
    import progressBar from './component/progress-bar/progress-bar.vue'
    import volumePanel from './component/volume-panel/volume-panel.vue'
    import ratePanel from './component/rate-panel/rate-panel.vue'
    import fullScreen from './component/full-screen/full-screen.vue'
    import Bus from './bus.js';
    import './assets/base.css'
    export default {
        data() {
            return {
                show: true,
                playStatus: false,
                noRatePanel: true,
                progress: 0,
                volumeStatus: true,
                volumeProgress: 25,
                tempVolume: 0,
                screenStatus: false,
                player: {},
                errorTag: false,
                currentTime: 0,
                duration: 0,
                timer: {
                    mousemoveTimer: null,
                    clickTimer: null
                },
                event: null,
            };
        },

        props: {
            options: {
                type: Object
            },
            type: {
                type: String,
            },
            src: {
                type: String
            },
            rate: {
                type: Number,
                default: 1
            }
        },

        components: {
            playPause,
            showTimes,
            progressBar,
            volumePanel,
            ratePanel,
            fullScreen
        },

        computed: {},

        mounted() {
            this.initEvent();
        },

        beforeDestroy() {
            Bus.$off('controlScreen');
        },

        methods: {
            initEvent() {
                const vm = this;
                this.$refs.video.addEventListener('play', function (e) {
                    vm.event = e;
                    vm.playStatus = true;
                });
                this.$refs.video.addEventListener('pause', function (e) {
                    vm.event = e;
                    vm.playStatus = false;
                })
                this.$refs.video.addEventListener('error', function (e) {
                    vm.progress = 0;
                    vm.$refs.video.poster = '';
                    vm.errorTag = true;
                    console.log(vm.$refs.video.error);
                });
                this.$refs.video.addEventListener('loadeddata', function (e) {
                    vm.$emit('on-loadeddata', e, vm.$refs.video);
                });
                this.$refs.video.addEventListener('durationchange', function(e) {
                    vm.duration = vm.$refs.video.duration;
                    
                });
                this.$refs.video.addEventListener('timeupdate', function(e) {
                    vm.currentTime = vm.$refs.video.currentTime;
                    vm.progress = vm.currentTime / vm.duration * 100;
                    vm.$emit('on-timeupdate', e, vm.$refs.video);
                });
                this.$refs.video.addEventListener('ended', function(e) {
                    vm.progress = 0;
                    vm.currentTime = 0;
                    vm.playStatus = false;
                    vm.$emit('on-ended', e, vm.$refs.video);
                });
                this.initAttr();
            },
            // 初始化video属性
            initAttr() {
                const vm = this;
                const {
                    autoplay = false,
                    muted = false,
                    loop = false,
                    preload = 'auto',
                    poster = '',
                    noRatePanel = true,
                } = this.options;
                // 是否自动播放
                if (autoplay) {
                    this.playStatus = true;
                }
                this.$refs.video.autoplay = autoplay;
                // 是否静音
                if (muted) {
                    this.volumeStatus = false;
                    this.tempVolume = 25;
                    this.volumeProgress = 0;
                } else {
                    this.volumeChange(25);
                }
                this.$refs.video.muted = muted;
                this.$refs.video.loop = loop;
                this.$refs.video.preload = preload;
                this.$refs.video.poster = poster;
                this.$refs.video.src = this.src;
                this.noRatePanel = noRatePanel;
                // 退出全屏
                document.addEventListener("webkitfullscreenchange", vm.screenResize);
            },
            // 控制底部栏，非全屏状态下不隐藏，全屏状态下3.8秒后隐藏
            controllBottomNav($event) {
                if (!this.screenStatus) {
                    this.show = true;
                    return;
                }
                this.show = true;
                this.$refs.videoBox.classList.remove('full-screen-noNav');
                this.$refs.videoBox.classList.remove('full-screen');
                this.$refs.controlNav.classList.remove('full-screen');
                this.$refs.videoBox.classList.add('full-screen');
                this.$refs.controlNav.classList.add('full-screen');
                clearTimeout(this.timer.mousemoveTimer);
                this.timer.mousemoveTimer = setTimeout(() => {
                    if ($event.path[$event.path.length - 7].className.indexOf('controlNav') == -1) {
                        if (this.screenStatus) {
                            this.show = false;
                            this.$refs.videoBox.classList.remove('full-screen');
                            this.$refs.videoBox.classList.add('full-screen-noNav');
                        }
                    }
                    clearTimeout(this.timer.mousemoveTimer);
                }, 3800)
            },
            // 全屏或退出全屏时的监听
            screenResize() {
                const isFull = document.fullscreenEnabled || window.fullScreen || document.webkitIsFullScreen ||
                    document.msFullscreenEnabled;
                if (!isFull) {
                    this.$refs.controlNav.classList.remove('full-screen');
                    this.$refs.videoBox.classList.remove('full-screen');
                    this.$refs.videoBox.classList.remove('full-screen-noNav');
                    this.screenStatus = !this.screenStatus;
                    this.$refs.videoBox.classList.remove('full-screen');
                }
            },
            // 暂停
            pause() {
                const vm = this;
                clearTimeout(this.timer.clickTimer);
                this.timer.clickTimer = setTimeout(() => {
                    if (vm.playStatus) {
                        vm.$refs.video.pause();
                        vm.$emit('on-pause', vm.event, vm.$refs.video);
                    }
                }, 380);
            },
            play() {
                this.$refs.video.play();
                this.$emit('on-start', this.event, this.$refs.video);
            },
            // 控制播放
            togglePlay() {
                if (this.errorTag) {
                    return;
                }
                if (!this.playStatus) {
                    this.play();
                } else {
                    this.pause();
                }
            },
            // 控制静音
            toggleVolume() {
                if (this.errorTag) {
                    return;
                }
                this.volumeStatus = !this.volumeStatus;
                if (!this.volumeStatus) {
                    this.tempVolume = this.volumeProgress;
                    this.volumeChange(0);
                } else {
                    this.volumeChange(this.tempVolume);
                }
            },
            dbclickToggleScreen() {
                clearTimeout(this.timer.clickTimer);
                this.toggleScreen();
            },
            // 控制全屏
            toggleScreen() {
                if (this.errorTag) {
                    return;
                }
                Bus.$emit('controlScreen', !this.screenStatus);
                if (!this.screenStatus) {
                    this.screenStatus = !this.screenStatus;
                    this.$refs.videoBox.classList.add('full-screen');
                    this.$refs.controlNav.classList.add('full-screen');
                }
            },
            progressBarChange(value) {
                if (!this.playStatus) {
                    this.togglePlay();
                }
                this.progress = value;
                this.$refs.video.currentTime = this.progress / 100 * this.$refs.video.duration;
            },
            volumeChange(value) {
                if (!this.volumeStatus && value != 0) {
                    this.volumeStatus=true;
                }
                this.$refs.video.muted = false;
                this.volumeProgress = value;
                this.$refs.video.volume = value / 100;
            },
            rateChange(value) {
                this.$emit('on-rate-change', value, this.$refs.video);
            }
        },
        watch: {
            src(val) {
                this.$refs.video.src = val;
                this.$refs.video.currentTime = this.currentTime;
                if (this.playStatus) {
                    this.$refs.video.play();
                }
                this.errorTag = false;
            }
        }
    }

</script>
<style lang='scss' scoped>
    @import './d-video.scss';
</style>
