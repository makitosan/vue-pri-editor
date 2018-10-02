<template>
  <div class="VuePriEditor">
    <h1>{{ msg }}</h1>
    <div class="VuePriEditor_Preview">
      <img id="preview" class="VuePriEditor_Preview_Image" :src="imgBase64" @click="stamp"/>
      <div class="VuePriEditor_Preview_Controllers">
        <button @click="grayscale">GRAY</button>
      </div>
      <div class="VuePriEditor_Preview_Stamps">
        <img v-for="item in stamps" :src="item.src" alt="item.name" @click="selectStamp(item)"/>
      </div>
      <div>
        <p>{{selectedStamp}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import Jimp from 'jimp/browser/lib/jimp'

export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    img: String,
    stamps: Array
  },
  data: function() {
    return {
      imgBase64: "",
      debug: "",
      selectedStamp: "",
      namedStamps: {}
    }
  },
  watch: {
    img: function(newVal, oldVal) { // watch it
      console.log('img changed')
      this.imgBase64 = newVal
    }
  },
  created: function() {
    this.stamps.forEach(function(item) {
      let url = item.src.replace(/^data:image\/\w+;base64,/, "");
      let buffer = new Buffer(url, 'base64');
      // https://github.com/oliver-moran/jimp/issues/231
      Jimp.read(buffer.buffer).then(function (tmpImage) {
        this.$set(this.namedStamps, item.name, tmpImage)
      }.bind(this)).catch(function (err) {
        console.error(err)
      });
    }.bind(this))
  },
  methods: {
    grayscale: function() {
      console.log('grayscale effect start')
      let url = this.imgBase64.replace(/^data:image\/\w+;base64,/, "");
      let buffer = new Buffer(url, 'base64');
      // https://github.com/oliver-moran/jimp/issues/231
      Jimp.read(buffer.buffer).then(function (tmpImage) {
        tmpImage.greyscale().getBase64(Jimp.MIME_PNG, function (err, src) {
          this.imgBase64 = src
        }.bind(this));
      }.bind(this)).catch(function (err) {
        console.error(err)
      });
    },
    selectStamp: function(item) {
      this.selectedStamp = item.name
    },
    stamp: function(e) {
      let url = this.imgBase64.replace(/^data:image\/\w+;base64,/, "");
      let buffer = new Buffer(url, 'base64');
      // https://github.com/oliver-moran/jimp/issues/231
      Jimp.read(buffer.buffer).then(function (tmpImage) {
        tmpImage.composite(this.namedStamps[this.selectedStamp], e.offsetX - this.namedStamps[this.selectedStamp].bitmap.width / 2, e.offsetY - this.namedStamps[this.selectedStamp].bitmap.height / 2)
          .getBase64(Jimp.MIME_PNG, function (err, src) {
          this.imgBase64 = src
        }.bind(this));
      }.bind(this)).catch(function (err) {
        console.error(err)
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
