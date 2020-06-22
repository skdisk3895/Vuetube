<template>
  <div class="text-center mt-4">
      <div v-if="Object.keys(video).length">
        <VideoDetail :video="video" />   
      </div>
      <div v-else>
        <h2>영화 예고편 검색</h2>
        <VideoSearch @input-text="getInputValue" />
        <VideoItem  @video-detail="getVideoDetail" :videos="videos" />
      </div>
  </div>
</template>

<script>
import axios from 'axios'
import VideoSearch from '@/components/VideoSearch.vue'
import VideoItem from '@/components/VideoItem.vue'
import VideoDetail from '@/components/VideoDetail.vue'

const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
const API_URL = 'https://www.googleapis.com/youtube/v3/search'

export default {
    name: 'VideoView',
    components: {
        VideoSearch,
        VideoDetail,
        VideoItem,
    },
    data() {
        return {
            inputValue: '',
            videos: [],
            video: {},
        }
    },
    Created () {
        console.log('create!!')
    },
    methods: {
        getInputValue(inputText) {
            this.inputValue = inputText + ' trailer'
            axios.get(API_URL, {
                params: {
                    key: API_KEY,
                    part: 'snippet',
                    type: 'video',
                    q: this.inputValue
                }
            })
                .then(res => this.videos = res.data.items)
        },
        getVideoDetail(movie) {
            this.video = movie
        }
    }
}
</script>

<style>

</style>