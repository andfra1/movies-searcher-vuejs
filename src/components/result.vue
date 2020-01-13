<template>
    <ul v-if="searchResult.length > 0">
        <li :key="index"
            v-for="(movie, index) in searchResult">
            <strong>{{movie.title}}</strong>
            , premiera: {{movie.release_date}}, || {{movie.popularity}} || {{ movie.vote_average}}
        </li>
    </ul>
    <p v-else-if="isLoading.status">{{isLoading.notice}}</p>
    <p v-else-if="hasError.status">{{hasError.notice}}</p>
    <p v-else-if="typedPhrase.length<=0">Start typing.</p>
    <p v-else-if="typedPhrase.length>0 && typedPhrase.length<3">Keep typing...</p>
    <p v-else-if="searchResult.length <= 0 && typedPhrase.length > 0">Nothing found.</p>
</template>

<script>
    export default {
        name: "result",
        props: ['searchResult','isLoading','hasError', 'typedPhrase'],
        watch: {
            searchResult: function(val){
                this.$emit('emit-status', val)
            }
        }
    }
</script>

<style scoped>

</style>