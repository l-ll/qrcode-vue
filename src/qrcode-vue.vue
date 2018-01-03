<template>
  <div>
    <img :src="dataUrl">
    <canvas ref="canvas" v-show="false"></canvas>
  </div>
</template>

<script>
  import qr from 'qr.js'
  export default {
    data () {
      return {
        dataUrl: ''
      }
    },
    props: {
      size: {
        type: Number
      },
      bgColor: {
        type: String,
        default: '#fff'
      },
      fgColor: {
        type: String,
        default: '#000'
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
        if (!this.value) {
          return;
        }
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
          image.setAttribute("crossOrigin",'Anonymous')
          image.onload = () => {
            var dwidth = this.size * 0.2
            var dx = (this.size - dwidth) / 2
            var dheight = image.height / image.width * dwidth
            var dy = (this.size - dheight) / 2
            image.width = dwidth
            image.height = dheight
            ctx.drawImage(image, dx, dy, dwidth, dheight)
            this.dataUrl = canvas.toDataURL()
          }
        }
      },
      getPixelRatio (ctx) {
        return ctx.webkitBackingStorePixelRatio || ctx.backingStorePixelRatio || 1
      }
    },
    mounted () {
      this.update()
    },
    watch: {
      value(val, oldValue) {
        this.update();
      },
    },
  }
</script>
