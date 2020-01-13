<template>
    <div>
        <label for="searcher">Type movie title:</label>
        <input id="searcher"
               type="text"
               v-model="typedPhrase"
               placeholder="type somtn'..."
               @keyup="findMovie(typedPhrase)">
        <button @click="findMovie(typedPhrase)">search</button>
    </div>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'

    Vue.use(VueAxios, axios);

    export default {
        name: "searcher",
        props: ['defaultMin','defaultMax'],
        data() {
            return {
                typedPhrase: '',
                searchResult: [],
                isLoading: {
                    status: false,
                    notice: 'Loading...'
                },
                hasError: {
                    status: false,
                    notice: ''
                }
            }
        },
        methods: {
            findMovie() {
                this.$emit('emit-typed-phrase', this.typedPhrase);
                this.searchResult = [];
                this.$emit('emit-search-result', this.searchResult)
                if (this.typedPhrase.length < 3) {
                    this.isLoading.status = false;
                    return
                }
                this.isLoading.status = true;
                this.$emit('emit-loading-status', this.isLoading)
                const apikey = '22c4e1afa48b120f97ec997bb4e815db';
                const baseURI = 'https://api.themoviedb.org/3/search/movie?api_key=' +
                    apikey + '&language=en-US&query=' +
                    this.typedPhrase + '&page=1';
                this.$http.get(baseURI)
                    .then((result) => {
                        //sortuj po popularnosci
                        let fetchedData = result.data.results;
                        if (fetchedData.length > 0) {
                            fetchedData.sort((a, b) => {
                                return b.popularity - a.popularity;
                            });
                            //jesli wybrano min rate
                            if (this.defaultMin) {
                                fetchedData.forEach((item) => {
                                    if (item.vote_average >= this.defaultMin && item.vote_average <= this.defaultMax) {
                                        this.searchResult.push(item)
                                        this.$emit('emit-search-result', this.searchResult)
                                    }
                                })
                            } else {
                                this.searchResult = fetchedData;
                                this.$emit('emit-search-result', this.searchResult)
                            }
                        }
                        //ustaw status
                        this.isLoading.status = false;
                        this.$emit('emit-loading-status', this.isLoading)
                    })
                    //jesli error
                    .catch(function (error) {
                        this.hasError.notice = error
                        this.$emit('emit-error-notice', this.hasError)
                    })
            }
        }
    }
</script>

<style scoped>

</style>