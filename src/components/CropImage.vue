<template>
  <section class="crop_image_wrapper">
    <div class="before">
      <canvas
        id="canvas"
        ref="canvas"
        class="canvas_elem"
      />
      <img
        src="@/assets/neko.jpeg"
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
      src="@/assets/neko.jpeg"
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
      ctx: null,
      draw: false,
      canvasX: null,
      canvasY: null,
      x1: null,
      y1: null,
      x2: null,
      y2: null,
      size: 500,
      img: null,
      canvasForCropImage: null,
      w: null,
      h: null,
      imgUrl: null,
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

      const threshold = 10

      if (Math.abs(this.x2 - this.x1) > threshold && Math.abs(this.y2 - this.y1) > threshold) {
        this.drawCanvas(this.x1, this.y1, this.x2 - this.x1, this.y2 - this.y1)
      }
    },
    drawCanvas() {
      this.$refs.cropImage.innerHTML = ''
      this.canvasForCropImage = this.$refs.cropCanvas
      this.img = document.querySelector('.neko_img_original')
      this.canvasForCropImage.width = 500
      this.canvasForCropImage.height = 500
      // this.img.addEventListener('load', this.imgLoadHandler)
      this.imgLoadHandler()

    },
    imgLoadHandler() {
      this.w = 500
      this.h = 500
      const RATE = this.img.width / 500

      const width = this.x2 - this.x1
      const height = this.y2 - this.y1

      this.canvasForCropImage.getContext('2d').drawImage(this.img, this.x1 * RATE, this.y1 * RATE, width * RATE, height * RATE, (this.w - this.w * 0.9) / 2, (this.h - this.h * 0.9) / 2, this.w * 0.9, this.h * .9)
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
	width: 100vw;
	height: 100vh;
	display: flex;
	justify-content: space-around;
	align-items: center;
}

.before, .after{
  border: 1px solid black;
  flex-direction:column;
  align-items: center;
  width:500px;
}

.canvas_elem {
	position: absolute;
}

.neko_img, .after_image {
	width: 500px;
	height: 500px;
}

.preview_wrapper {
	border: 1px solid;
	width: 200px;
	height: 200px;
	display: flex;
	justify-content: center;
	align-items: center;
}

.preview {
	width: 100%;
	height: 100%;
}

.neko_img_original {
	display: none;
	object-fit: contain;
	width: 100%;
	height: 100%;
}
</style>
