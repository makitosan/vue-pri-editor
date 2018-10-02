<template>
  <div class="VuePriEditor">
    <h1>{{ msg }}</h1>
    <div class="VuePriEditor_Preview">
      <img id="preview" class="VuePriEditor_Preview_Image" :src="imgBase64" @click="stamp"/>
      <div class="VuePriEditor_Preview_Controllers">
        <button @click="greyscale">GRAY</button>
        <button @click="sepia">SEPIA</button>
        <button @click="posterize">POSTERIZE</button>
        <button @click="blur">BLUR</button>
        <button @click="undo">UNDO </button>
        <button @click="showHistory" v-if="debugMode">HISTORY</button>
      </div>
      <div class="VuePriEditor_Preview_Stamps">
        <img v-for="item in stamps" :src="item.src" v-bind:class="{ selected: selectedStamp == item.name }" alt="item.name" @click="selectStamp(item)"/>
      </div>
      <div v-if="debugMode">
        <p >{{selectedStamp}}</p>
      </div>
      <div id="historyArea" v-if="debugMode"></div>
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
    stamps: Array,
    debugMode: {type: Boolean, default: false}
  },
  data: function() {
    return {
      imgBase64: "",
      jimpImage: null,
      debug: "",
      selectedStamp: "",
      namedStamps: {},
      history: []
    }
  },
  watch: {
    img: function(newVal, oldVal) { // watch it
      this.imgBase64 = newVal
      if(newVal != "" && newVal != null) {
        let url = this.imgBase64.replace(/^data:image\/\w+;base64,/, "");
        let buffer = new Buffer(url, 'base64');
        // https://github.com/oliver-moran/jimp/issues/231
        Jimp.read(buffer.buffer).then(function (tmpImage) {
          this.jimpImage = tmpImage
          // initialize history
          this.history.length = 0
        }.bind(this)).catch(function (err) {
          console.error(err)
        });
      }
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
    greyscale: function() {
      this.history.push(this.jimpImage.clone())
      this.jimpImage.greyscale().getBase64(Jimp.MIME_PNG, function (err, src) {
        this.imgBase64 = src
      }.bind(this));
    },
    sepia: function() {
      this.history.push(this.jimpImage.clone())
      this.jimpImage.sepia().getBase64(Jimp.MIME_PNG, function (err, src) {
        this.imgBase64 = src
      }.bind(this));
    },
    posterize: function() {
      this.history.push(this.jimpImage.clone())
      this.jimpImage.posterize(3).getBase64(Jimp.MIME_PNG, function (err, src) {
        this.imgBase64 = src
      }.bind(this));
    },
    blur: function() {
      this.history.push(this.jimpImage.clone())
      this.jimpImage.blur(3).getBase64(Jimp.MIME_PNG, function (err, src) {
        this.imgBase64 = src
      }.bind(this));
    },
    selectStamp: function(item) {
      this.selectedStamp = item.name
    },
    stamp: function(e) {
      this.history.push(this.jimpImage.clone())
      this.jimpImage.composite(this.namedStamps[this.selectedStamp], e.offsetX - this.namedStamps[this.selectedStamp].bitmap.width / 2, e.offsetY - this.namedStamps[this.selectedStamp].bitmap.height / 2)
        .getBase64(Jimp.MIME_PNG, function (err, src) {
        this.imgBase64 = src
      }.bind(this));
    },
    undo: function() {
      if(this.history.length > 0) {
        this.jimpImage = this.history.pop()
        this.jimpImage.getBase64(Jimp.MIME_PNG, function (err, src) {
          this.imgBase64 = src
        }.bind(this))
      }
    },
    showHistory: function() {
      this.history.forEach(function(item){
        item.getBase64(Jimp.MIME_PNG, function (err, src) {
          let img = document.createElement('img')
          img.src = src
          let historyArea = document.getElementById('historyArea')
          historyArea.appendChild(img)
        })
      })
    }
  }
}
</script>
<style lang="scss">
$stamp_border_width: 5px;

.VuePriEditor {
  background-color: #EEEEEE;
  &_Preview {
    &_Stamps {
      img {
        border: solid $stamp_border_width transparent;
      }
      img.selected {
        border: solid $stamp_border_width #cccccc;
      }
    }
  }
}
</style>
