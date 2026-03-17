  <script setup>

  import { ref, onMounted, onBeforeUnmount } from 'vue'
  import videojs from 'video.js'
  import 'video.js/dist/video-js.css'

  const lecteur = ref(null) // Lecteur reference pour le HTML
  const instancePlayer = ref(null) // Instance videojs du lecteur

  ///////////////////
  // SPEED SECTION //
  ///////////////////

  const actualSpeed = ref(1)

  const setSpeed = (speed) => {
    if(instancePlayer.value){
      instancePlayer.value.playbackRate(speed); // -> changement de vitesse
      actualSpeed.value = speed;
      //On s'assure que le pitch est preserve a chaque changement
      const videoElement = instancePlayer.value.el().querySelector('video');
      if(videoElement){
        videoElement.preservesPitch = true;
      }
    }
  }

  ///////////////////
  // LOOP  SECTION //
  ///////////////////

  const pointA = ref(null)
  const pointB = ref(null)
  const loopActive = ref(false)
  const autoAccelerate = ref(null)

  const setPointA = () => {
    pointA.value = instancePlayer.value.currentTime()
  }

  const setPointB = () => {
    const currentTime = instancePlayer.value.currentTime()
    if(pointA.value !== null && pointA.value >= currentTime){
      alert("Le point B doit être après le point A !")
      return
    }
    pointB.value = currentTime
    loopActive.value = true
  }

  const clearLoop = () => {
    pointA.value = null
    pointB.value = null
    loopActive.value = false
  }

  ///////////////////
  // INIT  SECTION //
  ///////////////////

  onMounted(() => {
    // Initialisation des options
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

    // Loop listener
    instancePlayer.value.on('timeupdate', () => {
      if(loopActive.value && pointA.value!==null && pointB.value!==null){
        const currentTime = instancePlayer.value.currentTime()
        if(currentTime>=pointB.value){
          instancePlayer.value.currentTime(pointA.value)
        }

      }
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

  <div class="controls">
    <button @click="setSpeed(0.5)">x0.5 (Lent)</button>
    <button @click="setSpeed(1)">x1 (Normal)</button>
    <button @click="setSpeed(1.5)">x1.5 (Rapide)</button>
    
    <p>Vitesse actuelle : {{ actualSpeed }}x</p>
  </div>

  <div class="loop-container">
    <button @click="setPointA">Point A</button>
    <button @click="setPointB">Point B</button>
    <button @click="clearLoop">Reset loop</button>

    <p>Point A : {{ pointA }}</p>
    <p>Point B : {{ pointB }}</p>
    <p>Loopactive : {{ loopActive }}</p>
  </div>
</template>