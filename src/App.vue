<template>
  <div id="app">
    <crop-image />
  </div>
</template>

<script>
import CropImage from './components/CropImage.vue'
export default {
  components: { CropImage, },
}
</script>

<style lang="scss">
* {
	margin: 0;
	padding: 0;
}
</style>
