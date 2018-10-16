<!--  -->
<template>
    <div class="volume">
        <Icon type="md-volume-up" v-if="volumeStatus" @click="toggleVolume" />
        <Icon type="md-volume-off" v-if="!volumeStatus" @click="toggleVolume" />
        <Slider 
            :value="value" 
            style="width:100%;margin-left:0.5em;"
            @on-change="progressBarChange"></Slider>
    </div>
</template>

<script>
    import { Slider } from 'iview';
    export default {
        data () {
            return {
            };
        },

        props: {
            value: {
                type: Number
            },
            volumeStatus: {
                type: Boolean
            }
        },

        components: { Slider },

        computed: {},

        mounted() {},

        methods: {
            progressBarChange(value) {
                this.$emit('on-progress-bar-change', value);
            },
            toggleVolume() {
                this.$emit('on-toggle-volume');
            }
        }
    }

</script>
<style lang='scss' scoped>
@import './volume-panel.scss';
</style>