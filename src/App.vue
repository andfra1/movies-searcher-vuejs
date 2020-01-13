<template>
    <div id="app">
        <Searcher :defaultMin="defaultMin"
                  :defaultMax="defaultMax"
                  @emit-search-result="getSearchResult"
                  @emit-loading-status="getLoadingStatus"
                  @emit-error-notice="getErrorNotice"
                  @emit-typed-phrase="getTypedPhrase"></Searcher>
        <Filters @emit-selected-rate-min="getSelectedRateMin"
                 @emit-selected-rate-max="getSelectedRateMax"></Filters>
        <Result :searchResult="searchResult"
                :isLoading="isLoading"
                :hasError="hasError"
                :typedPhrase="typedPhrase"></Result>
    </div>
</template>

<script>
    import Searcher from './components/searcher.vue'
    import Filters from './components/filters.vue'
    import Result from './components/result.vue'

    export default {
        name: 'app',
        components: {
            Result,
            Searcher, Filters
        },
        data() {
            return {
                currentPhrase: '',
                defaultMin: 0,
                defaultMax: 10,
                searchResult: [],
                isLoading: {
                    status: false,
                    notice: 'Loading...'
                },
                hasError: {
                    status: false,
                    notice: ''
                },
                typedPhrase: ''
            }
        },
        methods: {
            getSelectedRateMin(val) {
                this.defaultMin = val
            },
            getSelectedRateMax(val) {
                this.defaultMax = val
            },
            getSearchResult(val) {
                this.searchResult = val
            },
            getLoadingStatus(val) {
                this.isLoading = val
            },
            getErrorNotice(val) {
                this.hasError = val
            },
            getTypedPhrase(val) {
                this.typedPhrase = val
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    ul {

    }

    li {
        text-align: left;
    }
</style>
