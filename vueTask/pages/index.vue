<template>
  <div class="container">
    <!-- Creating a simple ul for each channel and its recent videos -->
    <div class="list" v-for="(channelData, i) in channelVideosData" :key="i">
      <ul><b>{{Object.keys(channelData)[0]}}</b></ul>
      <li v-for="(video, j) in channelData[Object.keys(channelData)[0]]" :key="j">
        <nuxt-link :to="'/video/'+video.videoId">{{video.videoTitle}}</nuxt-link>
      </li>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  components: {},
  data() {
    return {
      channelUserNames: [
        'nprmusic',
        'childishgambinoch',
        'kanyewest',
        'triplejtv',
        'PowerfulJRE'
      ],
      channelVideosData: []
    }
  },
  async fetch() {
    // ? refactor
    this.channelUserNames.forEach(async channelUsername => {
      // Fetches a channel's data including its recent uploads playlist
      let channel = await axios
        .get(
          `https://www.googleapis.com/youtube/v3/channels?part=snippet%2CcontentDetails&forUsername=${channelUsername}&key=${process.env.API_KEY}`
        )
        .catch(err => console.log(err))
      console.log(channel)
      // Fetching the separate videos in that playlist
      let recentUploadsPlaylistId =
        channel.data.items[0].contentDetails.relatedPlaylists.uploads
      let channelRecentUploads = await axios
        .get(
          `https://www.googleapis.com/youtube/v3/playlistItems?part=snippet%2CcontentDetails&maxResults=5&playlistId=${recentUploadsPlaylistId}&key=${process.env.API_KEY}`
        )
        .catch(err => console.log(err))

      // Mapping the data to a simpler format containing 
      // only the required fields
      let channelTitle = channelRecentUploads.data.items[0].snippet.channelTitle
      this.channelVideosData.push({
        [channelTitle]: channelRecentUploads.data.items.map(
          item => {return {videoId: item.contentDetails.videoId, videoTitle: item.snippet.title}}
        )
      })
    })
  }
}
</script>

<style scoped>
.container {
  margin: 2rem auto;
  min-height: 100vh;
  /* display: flex;
  justify-content: space-between;
  align-items: flex-start;
  flex-wrap: wrap; */
  /* text-align: center; */
}

.list{
  margin-top: 2rem
}

.links {
  padding-top: 15px;
}
</style>
