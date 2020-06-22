<template>
  <div class="col-3 mt-4">
    <div class="card" style="width:200px; height: 420px">
      <img :src="movie.poster_url" alt="" width="100%" height="300px">
      <div class="card-body">
        <h5 class="card-title" style="font-size: 18px;">{{ movie.title }}</h5>
        <button data-toggle="modal" data-target="#movieDetail" @click="getMovieDetail(movie)" class="btn btn-info mt-2" style="font-size: 12px;">영화 정보 상세보기</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'MovieListItem',
    props: {
      movie: Object
    },
    methods: {
      getMovieDetail(movie) {
        this.$emit('movie-detail', movie)
      }
    }
}
</script>

<style>

</style>