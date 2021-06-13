<template>
  <div class="row pt-5">
    <div class="col">
      <h5>ソース</h5>
      <video class="mt-1" id="video" controls :src="blobURL" ref="video"></video>
      <input type="file" id="file" @change="setVideoSrc">
      <p v-if="is_playing">Video再生中</p>
      <p v-if="is_filterd">フィルターON</p>
    </div>
    <div class="col">
      <h5>Preview</h5>
      <canvas class="mt-1" id="canvas" ref="canvas"></canvas>
    </div>
    <div class="col">
      <h5>TEST ACTIONS</h5>
      <div class="row">
        <div class="col">
          <div class="btn-group-vertical mr-1">
            <button class="btn btn btn-primary" id="play" @click="playVideo">Play Preview</button>
            <button class="btn btn btn-danger" id="pause" @click="pauseVideo">Pause</button>
            <button class="btn btn btn-warning" id="warining" @click="exportVideo">Export</button>
          </div>
          <div class="btn-group-vertical">
            <button class="btn btn btn-success" id="addText">テキストオーバーレイ</button>
            <button class="btn btn btn-success" id="addFilter">イメージオーバーレイ</button>
            <button class="btn btn btn-info" id="addFilter" @click="is_filterd=!is_filterd">フィルター</button>
            <button class="btn btn btn-warning" id="addFilter">3秒の画像シーンを追加</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Editor',
    data: () => {
      return {
        is_filterd : false,
        timeline: {},
        blobURL: "",
        is_playing: false,
        current_video: null
      }
    },
    computed: {
      video: function () {
        return this.$refs.video;
      },
      canvas: function () {
        return this.$refs.canvas;
      },
      ctx: function () {
        return this.$refs.canvas.getContext('2d');
      }
    },
    watch: {
      is_playing: function (newPlaying) {
        if (newPlaying) {
          this.streamToCanvas(this.current_video);
        }
      }
    },
    mounted: () => {
      //console.log('mounted');
    },
    methods: {
      setVideoSrc: function (event) {
        this.blobURL = URL.createObjectURL(event.target.files[0]);
      },
      playVideo() {
        this.is_playing = true;
        this.video.play();
        this.current_video = this.video;
        this.streamToCanvas();
      },
      pauseVideo() {
        this.is_playing = false;
        this.video.pause();
        this.current_video = this.video;
      },
      streamToCanvas() {
        this.computeFrame()
      },
       computeFrame(fps = 30) {
          var wait = fps / 1000;
          if(this.is_playing) {
            if(this.is_filterd) {
              this.ctx.filter = 'grayscale(100%)';
            } else {
              this.ctx.filter = 'none';
            }
            this.ctx.drawImage(this.current_video,0,0,this.canvas.width,this.canvas.height)
          } else {
            return;
          }

          setTimeout(() => {
            this.computeFrame();
          }, wait);
      }
    }
  }
</script>