<!--  -->
<template>
    <div class="showTimes">
        {{ thisCurrentTime }} / {{ thisDuration }}
    </div>
</template>

<script>
    export default {
        data () {
            return {
            };
        },

        props: {
            currentTime: {
                type: Number
            },
            duration: {
                type: Number,
            }
        },

        components: {},

        computed: {
            thisCurrentTime() {
                return this.formatTime(this.currentTime);
            },
            thisDuration() {
                return this.formatTime(this.duration);                
            }
        },

        mounted() {},

        methods: {
            formatTime(time) {
                var timeStr = parseInt(time);
                var minute = parseInt(timeStr / 60);
                if (minute == 0) {
                    if (timeStr < 10) {
                        timeStr = "0" + timeStr;
                    }
                    minute = "00:" + timeStr;
                } else {
                    var timeStr = timeStr % 60;
                    if (timeStr < 10) {
                        timeStr = "0" + timeStr;
                    }
                    minute = minute + ":" + timeStr;
                }
                return minute;
            }
        }
    }

</script>
<style lang='scss' scoped>
@import './show-times.scss';
</style>