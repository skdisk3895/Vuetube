<template>
  <div id="movieDetail" class="modal" tabindex="-1" role="dialog">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{ movie.title }}</h5>
          <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <img :src="movie.poster_url" alt="" width="100%" height="400px">
          <hr>
          <div>
            <h2><span class="badge badge-success">{{ movie.user_rating }}</span></h2>
          </div>
          <p>{{ movie.overview }}</p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'MovieListItemModal',
    props: {
      movie: Object
    }
}
</script>

<style>

</style>