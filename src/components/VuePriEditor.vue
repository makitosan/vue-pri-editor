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
        <div class="VuePriEditor_Preview_Controllers_Logo">
          <button @click="logo('top', 'left')">TOP Left</button>
          <button @click="logo('top', 'right')">TOP RIGHT</button>
          <button @click="logo('bottom', 'left')">BOTTOM Left</button>
          <button @click="logo('bottom', 'right')">BOTTOM RIGHT</button>
        </div>
      </div>

      <div class="VuePriEditor_Preview_Effects">
        <ul class="VuePriEditor_Preview_Effects_Tab">
          <li v-for="tab in effects" :key="tab.title" @click="selectTab(tab)" v-bind:class="{ selected: tab.isActive }" class="VuePriEditor_Preview_Effects_Tab_Item">
            <section class="VuePriEditor_Preview_Effects_Tab_Item_Title">{{tab.title}}</section>
          </li>
        </ul>
        <div class="VuePriEditor_Preview_Effects_Panels">
          <section v-for="tab in effects" :id="tab.title" :key="tab.title" v-show="tab.isActive" class="VuePriEditor_Preview_Effects_Panel">
            <img v-for="item in tab.items" :key="item.name" :src="item.src" v-bind:class="{ selected: selectedStamp == item.name }" :alt="item.name" @click="selectStamp(item)"/>
          </section>
        </div>

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
    logoImage: String,
    effects: Array,
    debugMode: {type: Boolean, default: false}
  },
  data: function() {
    return {
      imgBase64: "",
      jimpImage: null,
      jimpLogo: null,
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
    this.effects.forEach((effect) => {
      if(effect.title === 'stamp') {
        effect.items.forEach((item) => {
          let url = item.src.replace(/^data:image\/\w+;base64,/, "");
          let buffer = new Buffer(url, 'base64');
          // https://github.com/oliver-moran/jimp/issues/231
          Jimp.read(buffer.buffer).then(function (tmpImage) {
            this.$set(this.namedStamps, item.name, tmpImage)
          }.bind(this)).catch(function (err) {
            console.error(err)
          })
        })
      }
    })

    // Load logo image to jimp
    console.log(this.logoImage)
    let bufferLogo = new Buffer(this.logoImage.replace(/^data:image\/\w+;base64,/, ""), 'base64');
    // https://github.com/oliver-moran/jimp/issues/231
    Jimp.read(bufferLogo.buffer).then(function (tmpImage) {
      this.jimpLogo = tmpImage
    }.bind(this)).catch(function (err) {
      console.error(err)
    });

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

    selectTab: function(tab) {
      this.effects.forEach((tab) => {tab.isActive = false})
      tab.isActive = true
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
    logo: function(v, h) {
      this.history.push(this.jimpImage.clone())
      let logoWidth = this.jimpLogo.bitmap.width
      let logoHeight = this.jimpLogo.bitmap.height
      let x = 0
      let y = 0
      if('bottom' === v) y = this.jimpImage.bitmap.height - logoHeight
      if('right' === h) x = this.jimpImage.bitmap.width - logoWidth

      this.jimpImage.composite(this.jimpLogo, x, y)
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

*,
*:after,
*:before {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  position: relative;
}

.VuePriEditor {
  background-color: #EEEEEE;
  &_Preview {
    &_Effects {
      &_Tab {
        color: #999;
        font-size: 14px;
        font-weight: 600;
        margin-right: 0;
        list-style: none;

        @media (min-width: 700px) {
          border: 0;
          align-items: stretch;
          display: flex;
          justify-content: flex-start;
          margin-bottom: -1px;
        }

        :not(:last-child) {
          border-bottom: dotted 1px #ddd;
        }

        :hover {
          color: #666;
        }

        .selected {
          color: #000;
        }

        ul {
          border: solid 1px #ddd;
          border-radius: 6px;
          margin-bottom: 5px;
        }

        &_Item {
          background-color: #fff;
          border: solid 1px #ddd;
          border-radius: 3px 3px 0 0;
          margin-right: .5em;
          transform: translateY(2px);
          transition: transform .3s ease;

          &_Title {
            align-items: center;
            color: inherit;
            display: flex;
            padding: .75em 1em;
            text-decoration: none;
          }
        }

        li.selected {
          border-bottom: solid 1px #fff;
          z-index: 2;
          transform: translateY(0);
        }

      }

      &_Panels {
        background-color: #fff;
        padding: 0;
      }
      &_Panel {
        @media (min-width: 700px) {
          border-top-left-radius: 0;
          background-color: #fff;
          border: solid 1px #ddd;
          border-radius: 0 6px 6px 6px;
          box-shadow: 0 0 10px rgba(0, 0, 0, .05);
          padding: 1em 1em;
          text-align: left;

        }
      }

      img {
        border: solid $stamp_border_width transparent;
      }
      img.selected {
        border: solid $stamp_border_width #cccccc;
      }

    }
  }
}


.tabs-component-tab.is-disabled * {
  color: #cdcdcd;
  cursor: not-allowed !important;
}


</style>
