<template>
  <div id="app">
    <crop-image />
    <image-cropper />
  </div>
</template>

<script>
import ImageCropper from './components/ImageCropper.vue'
import CropImage from './components/CropImage.vue'

export default {
  components: { ImageCropper, CropImage, },
}
</script>

<style lang="scss">
* {
	margin: 0;
	padding: 0;
}
</style>
