<template>
  <div class="container">
    <iframe
      id="player"
      type="text/html"
      width="640"
      height="390"
      v-bind:src="videoUrl"
      frameborder="0"
    ></iframe>
  </div>
</template>

<script>
export default {
    data(){
        return{
            videoUrl: `http://www.youtube.com/embed/${this.$route.params.id}?enablejsapi=1&origin=http://example.com`
        }
    },
    components: {
    }
}
</script>

<style scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.links {
  padding-top: 15px;
}
</style>
