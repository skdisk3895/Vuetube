<template>
  <div class="container">
    <div class="row">
        <MovieListItem
            @movie-detail="getMovieDetail"
            v-for="movie in movies"
            :key="movie.id"
            :movie="movie"
        />
    </div>
  </div>
</template>

<script>
import MovieListItem from '@/components/MovieListItem.vue'

export default {
    name: 'MovieList',
    components: {
        MovieListItem
    },
    props: {
        movies: Array,
    },
    methods: {
        getMovieDetail(movie) {
            // console.log(movie)
            this.$emit('movie-detail', movie)
        }
    }
}
</script>

<style>

</style>