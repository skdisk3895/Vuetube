<template>
  <div class="mt-5">
      <div @click="getMovieDetail(video)" class="mt-4 on-video" v-for="video in videos" :key="video.etag">
          {{ video.snippet.title }}
          <div>
            <img :src="video.snippet.thumbnails.default.url" alt="" width="40%">
          </div>
      </div>
  </div>
</template>

<script>
export default {
    name: 'VideoItem',
    props: {
        videos: Array
    },
    methods: {
        getMovieDetail(movie) {
            this.$emit('video-detail', movie)
        }
    }
}
</script>

<style>
.on-video {
    cursor: pointer;
}

.on-video:hover {
    background-color: lightgrey;
}
</style>