<template>
  <div>
    <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" :src="videoUrl" allowfullscreen></iframe>
    </div>
    <h2>{{ video.snippet.title }}</h2>
  </div>
</template>

<script>
export default {
    name: 'VideoDetail',
    props: {
        video: Object,
    },
    data() {
        return {
            baseUrl: 'https://www.youtube.com/embed/'
        }
    },
    computed: {
        videoUrl() {
            return this.baseUrl + this.video.id.videoId
        }
    }
}
</script>

<style>

</style>