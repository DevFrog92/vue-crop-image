<template>
  <div class="img-cropper-container">
    <div class="img-container">
      <img
        ref="image"
        src="@/assets/neko.jpeg"
        alt="preview image"
      >
    </div>
    <img
      :src="destination"
      alt="cropped image"
      class="img-preview"
    >
  </div>
</template>

<script>
import  Cropper from 'cropperjs'

export default {
  name: 'ImageCropper',
  props: {
    src: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      cropper: {},
      destination: {},
      image: {},
    }
  },
  mounted() {
    this.image = this.$refs.image
    this.cropper = new Cropper(this.image, {
      zoomable: true,
      scalable: true,
      movable: true,
      dragMode: 'move',
      aspectRatio: 1,
      crop: () => {
        const canvas = this.cropper.getCroppedCanvas()

        this.destination = canvas.toDataURL('image/png')
      },
    })
  },
}
</script>

<style scoped>
.img-cropper-container {
    display: flex;
}

.img-container {
  width: 60%;
  height: 100vh;
  margin-right: 1rem;
}

.img-preview {
  width: 200px;
  height: 200px;
  border: 1px solid;
}
</style>
