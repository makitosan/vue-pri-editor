<template>
  <div id="app">
    <Message :msg="ctrString"/>
    <img id="logo_img" alt="Vue logo" src="./assets/yuukiIMGL2254_TP_V.jpg">
    <div>
      <button @click="countup">click</button>
      <button @click="reset">reset</button>
    </div>
    <VuePriEditor msg="Welcome to Your Vue.js App da"
      :img="base64"
      :effects="effects"
      :logoImage="logo"/>
  </div>
</template>

<script>
import VuePriEditor from './components/VuePriEditor.vue'
import Message from './components/Message.vue'

export default {
  name: 'app',
  components: {
    VuePriEditor,
    Message
  },
  data: function() {
    return {
      ctr: 0,
      ctrString: "",
      base64: "",
      effects: [
        {title: 'stamp', isActive: true, items:[
          {name: 'smile', src: require('./assets/smile.png')},
          {name: 'cry', src: require('./assets/cry.png')},
          {name: 'thumbsup', src: require('./assets/thumbsup.png')}
        ]},
        {title: 'frame', isActive: false, items:[
          {name: '', src: require('./assets/smile.png')}
        ]}
      ],
      logo: require('./assets/smile.png')
    }
  },
  methods: {
    countup: function() {
      console.log(this.ctr)
      this.ctr += 1
      this.ctrString = this.ctr.toString()

      let logo_img = document.getElementById('logo_img')
      // reset data and set data
      this.base64 = ""
      this.$nextTick() // forcely update DOM
        .then(function () {
          this.base64 = this.imageToBase64(logo_img, 'image/png')
        }.bind(this))
    },
    reset: function() {
      this.base64 = ''
    },
    imageToBase64: function (img, mime_type) {
        // New Canvas
        var canvas = document.createElement('canvas');
        canvas.width  = img.width;
        canvas.height = img.height;
        // Draw Image
        var ctx = canvas.getContext('2d');
        ctx.drawImage(img, 0, 0);
        // To Base64
        return canvas.toDataURL(mime_type);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
