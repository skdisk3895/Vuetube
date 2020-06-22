<template>
  <div class="container mt-4 text-center">
      <h2>나만의 무비 컬렉션</h2>
      <MovieListItemModal :movie="movie" />
      <div v-if="movies">
        <MovieList class="mt-5" @movie-detail="getMovieDetail" :movies="movies" />
      </div>
      <div v-else>
        <button class="btn btn-info mt-2" @click="getMovies">영화 가져오기</button>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
import MovieList from '@/components/MovieList.vue'
import MovieListItemModal from '@/components/MovieListItemModal.vue'

export default {
    name: 'MovieView',
    components: {
        MovieList,
        MovieListItemModal,
    },
    data() {
        return {
            movies: '',
            movie: {},
        }
    },
    methods: {
        getMovies() {
            axios.get('http://www.json-generator.com/api/json/get/ceNyuXZmwi?indent=2')
                .then(res => this.movies = res.data)
        },
        getMovieDetail(movie) {
            this.movie = movie
        }
    },
    mounted() {
        
    }
}
</script>

<style>

</style>