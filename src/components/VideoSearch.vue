<template>
  <div class="mt-4 form-inline justify-content-center">
      <input class="form-control" @keypress.enter="onKeypressMoviePreview" v-model="inputText">
      <button class="btn btn-info" @click="onClickMoviePreview">검색</button>
  </div>
</template>

<script>
export default {
    name: 'VideoSearch',
    data() {
        return {
            inputText: ''
        }
    },
    methods: {
        onClickMoviePreview() {
            this.$emit('input-text', this.inputText)
        },
        onKeypressMoviePreview() {
            this.$emit('input-text', this.inputText)
        }
    },
}
</script>

<style>

</style>