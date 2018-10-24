<!--  -->
<template>
    <div class="progressBar">
        <Slider 
            :value="value" 
            @on-change="progressBarChange" 
            style="line-height: 1;width:100%;"></Slider>
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
            }
        },

        components: { Slider },

        computed: {},

        mounted() {},

        methods: {
            progressBarChange(value) {
                this.$emit('on-progress-bar-change', value);
            }
        }
    }

</script>
<style lang='scss' scoped>
@import './progress-bar.scss';
</style>