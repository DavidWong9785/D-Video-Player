<!--  -->
<template>
    <div class="controlStatus">
        <Icon type="md-play" v-if="!playStatus" style="color: orange" @click="togglePlay"></Icon>
        <Icon type="ios-pause" v-if="playStatus" @click="togglePlay"></Icon>
    </div>
</template>

<script>
    import { Icon } from 'iview';
    export default {
        data () {
            return {
                
            };
        },

        props: {
            playStatus: {
                type: Boolean
            }
        },

        components: { Icon },

        computed: {},

        mounted() {},

        methods: {
            togglePlay() {
                this.$emit('on-play-status-change');
            },
        }
    }

</script>
<style lang='scss' scoped>
@import './play-pause.scss';
</style>