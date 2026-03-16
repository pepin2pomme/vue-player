<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import videojs from 'video.js'
import 'video.js/dist/video-js.css'

const lecteur = ref(null)
const instancePlayer = ref(null)

onMounted(() => {
  instancePlayer.value = videojs(lecteur.value, {
    controls: true,
    autoplay: false,
    preload: 'auto',
    sources: [
      {
        src: 'https://vjs.zencdn.net/v/oceans.mp4',
        type: 'video/mp4'
      }
    ]
  })

  
})

onBeforeUnmount(() => {
  if(instancePlayer.value){
    instancePlayer.value.dispose()
  }
})
</script>

<template>
  <div class="player-container">
    <p>Video</p>
    <video 
      ref="lecteur" 
      class="video-js vjs-big-play-centered"
    ></video>
  </div>
</template>