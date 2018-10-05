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
          {name: 'u1f600', src: require('./assets/emoji/emoji_u1f600.png'), hash: ''},
          {name: 'u1f601', src: require('./assets/emoji/emoji_u1f601.png'), hash: ''},
          {name: 'u1f602', src: require('./assets/emoji/emoji_u1f602.png'), hash: ''},
          {name: 'u1f603', src: require('./assets/emoji/emoji_u1f603.png'), hash: ''},
          {name: 'u1f604', src: require('./assets/emoji/emoji_u1f604.png'), hash: ''},
          {name: 'u1f605', src: require('./assets/emoji/emoji_u1f605.png'), hash: ''},
          {name: 'u1f606', src: require('./assets/emoji/emoji_u1f606.png'), hash: ''},
          {name: 'u1f607', src: require('./assets/emoji/emoji_u1f607.png'), hash: ''},
          {name: 'u1f608', src: require('./assets/emoji/emoji_u1f608.png'), hash: ''},
          {name: 'u1f609', src: require('./assets/emoji/emoji_u1f609.png'), hash: ''},
          {name: 'u1f60a', src: require('./assets/emoji/emoji_u1f60a.png'), hash: ''},
          {name: 'u1f60b', src: require('./assets/emoji/emoji_u1f60b.png'), hash: ''},
          {name: 'u1f60c', src: require('./assets/emoji/emoji_u1f60c.png'), hash: ''},
          {name: 'u1f60d', src: require('./assets/emoji/emoji_u1f60d.png'), hash: ''},
          {name: 'u1f60e', src: require('./assets/emoji/emoji_u1f60e.png'), hash: ''},
          {name: 'u1f60f', src: require('./assets/emoji/emoji_u1f60f.png'), hash: ''},
          {name: 'u1f610', src: require('./assets/emoji/emoji_u1f610.png'), hash: ''},
          {name: 'u1f611', src: require('./assets/emoji/emoji_u1f611.png'), hash: ''},
          {name: 'u1f612', src: require('./assets/emoji/emoji_u1f612.png'), hash: ''},
          {name: 'u1f613', src: require('./assets/emoji/emoji_u1f613.png'), hash: ''},
          {name: 'u1f614', src: require('./assets/emoji/emoji_u1f614.png'), hash: ''},
          {name: 'u1f615', src: require('./assets/emoji/emoji_u1f615.png'), hash: ''},
          {name: 'u1f616', src: require('./assets/emoji/emoji_u1f616.png'), hash: ''},
          {name: 'u1f617', src: require('./assets/emoji/emoji_u1f617.png'), hash: ''},
          {name: 'u1f618', src: require('./assets/emoji/emoji_u1f618.png'), hash: ''},
          {name: 'u1f619', src: require('./assets/emoji/emoji_u1f619.png'), hash: ''},
          {name: 'u1f61a', src: require('./assets/emoji/emoji_u1f61a.png'), hash: ''},
          {name: 'u1f61b', src: require('./assets/emoji/emoji_u1f61b.png'), hash: ''},
          {name: 'u1f61c', src: require('./assets/emoji/emoji_u1f61c.png'), hash: ''},
          {name: 'u1f61d', src: require('./assets/emoji/emoji_u1f61d.png'), hash: ''},
          {name: 'u1f61e', src: require('./assets/emoji/emoji_u1f61e.png'), hash: ''},
          {name: 'u1f61f', src: require('./assets/emoji/emoji_u1f61f.png'), hash: ''},
          {name: 'u1f620', src: require('./assets/emoji/emoji_u1f620.png'), hash: ''},
          {name: 'u1f621', src: require('./assets/emoji/emoji_u1f621.png'), hash: ''},
          {name: 'u1f622', src: require('./assets/emoji/emoji_u1f622.png'), hash: ''},
          {name: 'u1f623', src: require('./assets/emoji/emoji_u1f623.png'), hash: ''},
          {name: 'u1f624', src: require('./assets/emoji/emoji_u1f624.png'), hash: ''},
          {name: 'u1f625', src: require('./assets/emoji/emoji_u1f625.png'), hash: ''},
          {name: 'u1f626', src: require('./assets/emoji/emoji_u1f626.png'), hash: ''},
          {name: 'u1f627', src: require('./assets/emoji/emoji_u1f627.png'), hash: ''},
          {name: 'u1f628', src: require('./assets/emoji/emoji_u1f628.png'), hash: ''},
          {name: 'u1f629', src: require('./assets/emoji/emoji_u1f629.png'), hash: ''},
          {name: 'u1f62a', src: require('./assets/emoji/emoji_u1f62a.png'), hash: ''},
          {name: 'u1f62b', src: require('./assets/emoji/emoji_u1f62b.png'), hash: ''},
          {name: 'u1f62c', src: require('./assets/emoji/emoji_u1f62c.png'), hash: ''},
          {name: 'u1f62d', src: require('./assets/emoji/emoji_u1f62d.png'), hash: ''},
          {name: 'u1f62e', src: require('./assets/emoji/emoji_u1f62e.png'), hash: ''},
          {name: 'u1f62f', src: require('./assets/emoji/emoji_u1f62f.png'), hash: ''},
          {name: 'u1f630', src: require('./assets/emoji/emoji_u1f630.png'), hash: ''},
          {name: 'u1f631', src: require('./assets/emoji/emoji_u1f631.png'), hash: ''},
          {name: 'u1f632', src: require('./assets/emoji/emoji_u1f632.png'), hash: ''},
          {name: 'u1f633', src: require('./assets/emoji/emoji_u1f633.png'), hash: ''},
          {name: 'u1f634', src: require('./assets/emoji/emoji_u1f634.png'), hash: ''},
          {name: 'u1f635', src: require('./assets/emoji/emoji_u1f635.png'), hash: ''},
          {name: 'u1f636', src: require('./assets/emoji/emoji_u1f636.png'), hash: ''},
          {name: 'u1f637', src: require('./assets/emoji/emoji_u1f637.png'), hash: ''},
          {name: 'u1f638', src: require('./assets/emoji/emoji_u1f638.png'), hash: ''},
          {name: 'u1f639', src: require('./assets/emoji/emoji_u1f639.png'), hash: ''},
          {name: 'u1f63a', src: require('./assets/emoji/emoji_u1f63a.png'), hash: ''},
          {name: 'u1f63b', src: require('./assets/emoji/emoji_u1f63b.png'), hash: ''},
          {name: 'u1f63c', src: require('./assets/emoji/emoji_u1f63c.png'), hash: ''},
          {name: 'u1f63d', src: require('./assets/emoji/emoji_u1f63d.png'), hash: ''},
          {name: 'u1f63e', src: require('./assets/emoji/emoji_u1f63e.png'), hash: ''},
          {name: 'u1f63f', src: require('./assets/emoji/emoji_u1f63f.png'), hash: ''}
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
