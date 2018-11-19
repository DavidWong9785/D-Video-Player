<!--  -->
<template>
    <div class="full-screen">
        <Icon type="md-contract" v-if="screenStatus" @click="toggleScreen" />
        <Icon type="md-expand" v-if="!screenStatus" @click="toggleScreen"  />
    </div>
</template>

<script>
    import { Icon } from 'iview';
    import Bus from '../../bus.js';
    export default {
        data () {
            return {
            };
        },

        props: {
            screenStatus: {
                type: Boolean
            }
        },

        components: { Icon },

        computed: {},

        mounted() {
            const vm = this;
            Bus.$on('controlScreen', (data) => {
                if (data) {
                    vm.fullScreen();
                } else {
                    vm.exitScreen();
                }
            })
        },

        methods: {
            toggleScreen() {
                this.$emit('on-full-cscreen-change');
            },
            fullScreen() {
                const el = document.documentElement;
                const rfs = el.requestFullScreen || el.webkitRequestFullScreen || el.mozRequestFullScreen || el.msRequestFullscreen;      
                if(typeof rfs != "undefined" && rfs) {
                    rfs.call(el);
                };
                return;
            },
            exitScreen() {
                if (document.exitFullscreen) {  
                    document.exitFullscreen();  
                }  
                else if (document.mozCancelFullScreen) {  
                    document.mozCancelFullScreen();  
                }  
                else if (document.webkitCancelFullScreen) {  
                    document.webkitCancelFullScreen();  
                }  
                else if (document.msExitFullscreen) {  
                    document.msExitFullscreen();  
                }
            }
        }
    }

</script>
<style lang='scss' scoped>
@import './full-screen.scss';
</style>