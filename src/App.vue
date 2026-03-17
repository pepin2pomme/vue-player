<script setup>
import videojs from 'video.js'
import 'video.js/dist/video-js.css'
import { Play, Pause, RotateCcw } from 'lucide-vue-next'

import player from './components/player.vue';
</script>

<template>
  <p>Lecteur Video</p>
  <player></player>
</template>
