<template>
  <div class="container">
    <!-- // TODO: add one channel with some of its videos (channelSections)-->
    <nuxt-link v-bind:to="temp">Video</nuxt-link>
  </div>
</template>

<script>
import axios from "axios";

export default {
  components: {
  },
  data () {
    return {
      channels: [],
      channelUserNames:["triplejtv", "nprmusic", "RiotGamesInc"],
      temp: "/videos"
    }
  },
  async fetch () {
    let channel = await axios.get(`https://www.googleapis.com/youtube/v3/channels?part=snippet%2CcontentDetails&forUsername=triplejtv&key=${process.env.API_KEY}`)
    .catch((err)=>console.log(err));
    console.log(channel);

    let recentUploadsPlaylistId = channel.data.items[0].contentDetails.relatedPlaylists.uploads;

    let channelRecentUploads = await axios.get(`https://www.googleapis.com/youtube/v3/playlistItems?part=contentDetails&maxResults=5&playlistId=${recentUploadsPlaylistId}&key=${process.env.API_KEY}`)
    .catch((err)=>console.log(err));
    console.log(channelRecentUploads);
    
    this.temp = "/video/"+channelRecentUploads.data.items[0].contentDetails.videoId
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

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
