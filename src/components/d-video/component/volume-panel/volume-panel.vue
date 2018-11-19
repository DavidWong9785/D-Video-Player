<!--  -->
<template>
    <div class="volume">
        <Icon type="md-volume-up" v-if="volumeStatus" @click="toggleVolume" />
        <Icon type="md-volume-off" v-if="!volumeStatus" @click="toggleVolume" />
        <Slider 
            :value="value" 
            style="line-height: 1;width:100%;margin-left:0.5em;"
            @on-change="progressBarChange"></Slider>
    </div>
</template>

<script>
    import { Slider, Icon } from 'iview';
    export default {
        data () {
            return {
            };
        },

        props: {
            value: {
                type: Number,
                default: 25
            },
            volumeStatus: {
                type: Boolean
            }
        },

        components: { Slider, Icon },

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