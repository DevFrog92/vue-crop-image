<template>
  <div id="app">
    <image-cropper />
  </div>
</template>

<script>
import ImageCropper from './components/ImageCropper.vue'
export default {
  components: { ImageCropper, },
}
</script>

<style lang="scss">
* {
	margin: 0;
	padding: 0;
}
</style>
