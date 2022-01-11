<template>
  <section class="crop_image_wrapper">
    <input
      id="input"
      ref="image"
      type="file"
      name="image"
      accept="image/*"
      multiple="multiple"
      class="hidden"
      @change="uploadImage()"
    >
    <div class="before">
      <canvas
        id="canvas"
        ref="canvas"
        class="canvas_elem"
      />
      <img
        :src="url"
        alt="neko"
        class="neko_img"
      >
    </div>
    <div class="after">
      <div
        ref="cropImage"
        class="after_image"
      >
        <canvas ref="cropCanvas" />
      </div>
    </div>
    <img
      :src="url"
      alt="neko"
      class="neko_img_original"
    >
    <button @click="save">
      save
    </button>
    <div class="preview_wrapper">
      <img
        class="preview"
      >
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      canvasForCropImage: null,
      canvasX: null,
      canvasY: null,
      imgUrl: null,
      draw: false,
      img: null,
      ctx: null,
      x1: null,
      y1: null,
      x2: null,
      y2: null,
      h: null,
      w: null,
      size: 500,
      url: null,
    }
  },
  mounted() {
    const canvas = this.$refs.canvas

    this.setCanvas(canvas)
    this.setMouseDownEvent(canvas)
    this.setMouseMoveEvent(canvas)
    this.setMouseUpEvent(canvas)
  },
  methods: {
    uploadImage() {
      let image = this.$refs['image'].files[0]

      this.url = URL.createObjectURL(image)

      console.log(image)
    },
    save() {
      const prevImage = document.querySelector('.preview')

      this.imgUrl = this.canvasForCropImage.toDataURL('image/png')
      prevImage.src = this.imgUrl
    },
    setMouseDownEvent(canvasElem) {
      canvasElem.addEventListener('mousedown', this.mousedownHandler)
    },
    setMouseMoveEvent(canvasElem) {
      canvasElem.addEventListener('mousemove', this.mousemoveHandler)
    },
    setMouseUpEvent(canvasElem) {
      canvasElem.addEventListener('mouseup', this.mouseupHandler)
    },
    mouseupHandler() {
      this.draw = false

      const threshold = 50

      if (Math.abs(this.x2 - this.x1) > threshold && Math.abs(this.y2 - this.y1) > threshold) {
        this.drawCanvas(this.x1, this.y1, this.x2 - this.x1, this.y2 - this.y1)
      }
    },
    drawCanvas() {
      this.$refs.cropImage.innerHTML = ''
      this.canvasForCropImage = this.$refs.cropCanvas
      this.canvasForCropImage.width = 500
      this.canvasForCropImage.height = 500
      this.img = document.querySelector('.neko_img_original')

      const RATE = this.img.width / 500
      const width = this.x2 - this.x1
      const height = this.y2 - this.y1
      const reduceRatio = 0.6

      this.canvasForCropImage.getContext('2d').drawImage(this.img, this.x1 * RATE, this.y1 * RATE, width * RATE, height * RATE, (this.size - this.size * reduceRatio) / 2, (this.size - this.size * reduceRatio) / 2, this.size * reduceRatio, this.size * reduceRatio)
      this.$refs.cropImage.append(this.canvasForCropImage)
    },
    mousedownHandler(e) {
      this.x1 = parseInt(e.clientX - this.canvasX)
      this.y1 = parseInt(e.clientY - this.canvasY)
      this.draw = true
    },
    mousemoveHandler(e) {
      if (!this.draw) {
        return
      }

      const canvasElem = this.$refs.canvas

      this.x2 = parseInt(e.clientX - this.canvasX)
      this.y2 = parseInt(e.clientY - this.canvasY)
      this.ctx.clearRect(0, 0, canvasElem.width, canvasElem.height)
      this.ctx.strokeRect(
        this.x1,
        this.y1,
        this.x2 - this.x1,
        this.y2 - this.y1
      )

      this.draw = true
    },
    setCanvas(canvasElem) {
      canvasElem.width = 500
      canvasElem.height = 500
      this.ctx = canvasElem.getContext('2d')
      this.canvasX = canvasElem.getBoundingClientRect().left
      this.canvasY = canvasElem.getBoundingClientRect().top
      this.setCanvasStroke(this.ctx)
    },
    setCanvasStroke(ctx) {
      ctx.lineWith = 3
      ctx.strokeStyle = 'red'
    },
  },
}
</script>

<style scoped>
.crop_image_wrapper {
  align-items: center;
  display: flex;
  height: 100vh;
  justify-content: space-around;
  width: 100vw;
}

.hidden {
	position: absolute;
	top: 10px;
}

.before, .after {
  align-items: center;
  border: 1px solid black;
  flex-direction: column;
  width: 500px;
	height: 500px;
}

.canvas_elem {
  position: absolute;
	z-index: 100;
}

.neko_img, .after_image {
  height: 500px;
  width: 500px;
}

.preview_wrapper {
  align-items: center;
  border: 1px solid;
  display: flex;
  height: 200px;
  justify-content: center;
  width: 200px;
}

.preview {
  height: 100%;
  width: 100%;
}

.neko_img_original {
  display: none;
  height: 100%;
  object-fit: contain;
  width: 100%;
}
</style>
