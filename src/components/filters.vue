<template>
    <div>
        <label for="rating-min">Min rating:</label>
        <select id="rating-min" v-model.number="defaultMin">
            <option v-for="(rate) in topRateValue"
                    :key="rate"
            >{{rate-1}}
            </option>
        </select>
        <label for="rating-max">Max rating:</label>
        <select id="rating-max" v-model.number="defaultMax">
            <option v-for="(rate) in topRateValue"
                    :key="rate"
            >{{rate-1}}
            </option>
        </select>
    </div>
</template>

<script>
    export default {
        name: "filters",
        data() {
            return {
                defaultMin: 0,
                defaultMax: 10,
                minRate: 0,
                maxRate: 10,
                topRateValue: 11,
            }
        },
        watch: {
            defaultMin: function (val) {
                this.$emit('emit-selected-rate-min', val)
                // return this.defaultMax > this.defaultMin ? this.defaultMin = this.defaultMin - 1 : this.defaultMax = val;
            },
            defaultMax: function (val) {
                this.$emit('emit-selected-rate-max', val)
                // return this.defaultMax < this.defaultMin ? this.defaultMin = val - 1 : this.defaultMin = this.defaultMax

            }
        }
    }
</script>

<style scoped>

</style>