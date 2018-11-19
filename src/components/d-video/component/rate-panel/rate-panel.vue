<!--  -->
<template>
    <div class="rate" @mouseover="showRatePanel" @mouseout="hiddenRatePanel">
        <span>{{ rateType[rate] }}</span>
        <div class="rate-select-panel" v-show="showRatePanelTag">
            <div class="rate-select-item" v-for="(item, index) in rateType">
                <p :class="index == rate? 'current' : ''" @click="changeRate(index)">{{ item }}</p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                rateType: ['超 清','高 清','流 畅'],
                currentRate: 1,
                showRatePanelTag: false,
            };
        },

        props: {
            rate: {
                type: Number
            }
        },

        components: {},

        computed: {},

        mounted() {},

        methods: {
            changeRate(index) {
                this.showRatePanelTag = false;
                this.$emit('on-rate-change', index);
            },
            showRatePanel() {
                this.showRatePanelTag = true;
            },
            hiddenRatePanel() {
                this.showRatePanelTag = false;
            },
        }
    }

</script>
<style lang='scss' scoped>
    @import './rate-panel.scss'

</style>
