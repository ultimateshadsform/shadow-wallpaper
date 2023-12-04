<template>
  <div
    class="bg-red-800 w-full h-full flex justify-center items-center overflow-hidden"
  >
    <img
      class="image"
      ref="audioImage"
      src="./assets/images/shadow.webp"
      :style="{
        transform: `scale(${windowWidth * 0.2 * 2 * MAX_RADIUS})`,
        width: `${windowWidth * 0.2 * 2 * MAX_RADIUS}px`,
        height: `${windowHeight * 0.2 * 2 * MAX_RADIUS}px`,
      }"
    />
  </div>
</template>

<style lang="scss">
html,
body,
#app {
  @apply w-full h-full;
}

.image {
  @apply object-contain transform transition-transform;
  filter: drop-shadow(0 0 1rem red);
}
</style>

<script setup lang="ts">
import { onMounted, ref } from 'vue';

const audioImage = ref<HTMLImageElement | null>(null);
const MAX_RADIUS = 3; // Maximum radius as a proportion of the canvas width
const windowWidth = ref(window.innerWidth);
const windowHeight = ref(window.innerHeight);

const wallpaperAudioListener = (audioArray: number[]) => {
  // Calculate the average audio level
  const averageAudioLevel = ref(0);
  for (const value of audioArray) {
    averageAudioLevel.value += value;
  }
  averageAudioLevel.value /= audioArray.length;

  // Update the image scale based on the average audio level
  const scaleFactor = MAX_RADIUS * averageAudioLevel.value;

  // Update the image width and height based on the scale factor and window dimensions
  const imageWidth = windowWidth.value * 0.2 * 2 * MAX_RADIUS;
  const imageHeight = windowHeight.value * 0.2 * 2 * MAX_RADIUS;

  audioImage.value!.style.transform = `scale(${scaleFactor})`;
  audioImage.value!.style.width = `${imageWidth}px`;
  audioImage.value!.style.height = `${imageHeight}px`;
};

// Update windowWidth and windowHeight whenever the window resizes
window.addEventListener('resize', () => {
  windowWidth.value = window.innerWidth;
  windowHeight.value = window.innerHeight;
});

// Get the audio image once the component is mounted
onMounted(() => {
  // Register the audio listener provided by Wallpaper Engine.
  window.wallpaperRegisterAudioListener(wallpaperAudioListener);
});
</script>
