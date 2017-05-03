<template>
  <div>
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
  import qr from 'qr.js'
  export default {
    props: {
      size: {
        type: Number
      },
      bgColor: {
        type: String
      },
      fgColor: {
        type: String
      },
      value: {
        type: String
      },
      logo: {
        type: String
      }
    },
    methods: {
      update () {
        const qrcode = qr(this.value)
        const canvas = this.$refs.canvas
        const ctx = canvas.getContext('2d')
        const size = this.size / qrcode.moduleCount
        const scale = window.devicePixelRatio / this.getPixelRatio(ctx)
        canvas.height = canvas.width = this.size * scale
        ctx.scale(scale, scale)
        qrcode.modules.forEach((row, rdx) => {
          row.forEach((cell, cdx) => {
            ctx.fillStyle = cell ? this.fgColor : this.bgColor
            var w = (Math.ceil((cdx + 1) * size) - Math.floor(cdx * size))
            ctx.fillRect(Math.round(cdx * size), Math.round(rdx * size), w, w)
          })
        })
        if (this.logo) {
          var image = document.createElement('img')
          image.src = this.logo
          image.onload = () => {
            var dwidth = this.size * 0.2
            var dx = (this.size - dwidth) / 2
            var dheight = image.height / image.width * dwidth
            var dy = (this.size - dheight) / 2
            image.width = dwidth
            image.height = dheight
            ctx.drawImage(image, dx, dy, dwidth, dheight)
          }
        }
      },
      getPixelRatio (ctx) {
        return ctx.webkitBackingStorePixelRatio || ctx.backingStorePixelRatio || 1
      }
    },
    mounted () {
      this.update()
    }
  }
</script>
