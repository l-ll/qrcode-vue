# qrcode-vue
qrcode vue logo

```html
<template>
  <div>
    <qrcode-vue 
      :size="size" 
      :value="value" 
      :logo="logo" 
      :bgColor="bgColor" 
      :fgColor="fgColor"
    ></qrcode-vue>
  </div>
</template>
```

```javascript
<script>
  import qrcodeVue from '../src/qrcode-vue'
  export default {
    data () {
      return {
        size: 128,
        bgColor: '#fff',
        fgColor: '#000',
        value: 'https://github.com/l-ll/qrcode-vue',
        logo: 'https://avatars3.githubusercontent.com/u/7104141?v=3&s=80'
      }
    },
    components: {
      qrcodeVue
    }
  }
</script>
```
