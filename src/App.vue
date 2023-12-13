<script setup lang="ts">
import { onMounted, ref } from 'vue';
import {
  WallpaperMediaPropertiesEvent,
  WallpaperMediaThumbnailEvent,
} from 'wallpaper-engine-types';

const media = ref<WallpaperMediaPropertiesEvent | null>(null);
const mediaImage = ref<string | null>(null);

const wallpaperMediaStatusListener = (event: WallpaperMediaPropertiesEvent) => {
  media.value = event;
};

function wallpaperMediaThumbnailListener(event: WallpaperMediaThumbnailEvent) {
  mediaImage.value = event.thumbnail;
}

// Get the audio image once the component is mounted
onMounted(() => {
  window.wallpaperRegisterMediaPropertiesListener(wallpaperMediaStatusListener);
  window.wallpaperRegisterMediaThumbnailListener(
    wallpaperMediaThumbnailListener
  );
});
</script>

<template>
  <div
    class="bg-red-800 w-full h-full flex justify-center items-center overflow-hidden"
  >
    <h1>Title: {{ media?.title }}</h1>
    <h1>Artist: {{ media?.artist }}</h1>
    <h1>subTitle: {{ media?.subTitle }}</h1>
    <h1>albumTitle: {{ media?.albumTitle }}</h1>
    <h1>albumArtist: {{ media?.albumArtist }}</h1>
    <h1>genres: {{ media?.genres }}</h1>
    <h1>contentType: {{ media?.genres }}</h1>
    <img v-if="mediaImage" :src="mediaImage" />
  </div>
</template>

<style lang="scss">
html,
body,
#app {
  @apply w-full h-full;
}
</style>
