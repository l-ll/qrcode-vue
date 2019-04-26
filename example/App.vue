<template>
  <div id="app">
    <qrcode-vue
      :size="size"
      :value="value"
      :bgColor="bgColor"
      :fgColor="fgColor"
    ></qrcode-vue>
    <input type="text" v-model="value">
  </div>
</template>

<script>
import qrcodeVue from '../lib/qrcode-vue'
  export default {
    data () {
      return {
        size: 128,
        bgColor: '#fff',
        fgColor: '#000',
        value: 'https://github.com/l-ll/qrcode-vue'
      }
    },
    components: {
      qrcodeVue
    }
  }
</script>
