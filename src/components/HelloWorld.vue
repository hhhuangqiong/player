<template>
  <div class="container">
    <video 
      id="video"
      class="video-player__video"
      width="300px"
      height="280px"
      src="https://www.w3school.com.cn/example/html5/mov_bbb.mp4"
      controls
      ref="videoPlayer"
      @canplay="canplay"
      @play="statePlay"
      @pause="statePause"
      @ended="stateEnded"
      @durationchange="durationchange"
      @timeupdate="timeupdate"
    ></video>
    <div class="video_control">
      <button @click="playVideo"> 
        <span v-if="videoState !== 'PLAY'">
          播放
        </span>
        <span v-else>
          暂停
        </span>
      </button>
      <div class="progress_bar">
        <div class="progress" ref="currentPlayProgress"> </div>
      </div>
      <div> {{ currentTime }} / </div>
      <div> {{ duration }} </div>
    </div>
  </div>
</template>

<script>

/**
 * 将时间从秒转成XX： XX：XX
 * @param {*} time
 */
function formatVideoTime(seconds) {
  let s = Math.floor(seconds % 60);
  let m = Math.floor(seconds / 60 % 60);
  let h = Math.floor(seconds / 3600);

  h = (h > 0) ? h + ':' : '';

  // If hours are showing, we may need to add a leading zero.
  // Always show at least one digit of minutes.
  m = ((m < 10) ? '0' + m : m) + ':';

  // Check if leading zero is need for seconds
  s = (s < 10) ? '0' + s : s;
  return h + m + s;
}

const PAUSE = 'PAUSE';
const PLAY = 'PLAY';
const ENDED = 'ENDED';
const PLAY_TOTAL_WIDTH = 150;

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      videoState: PAUSE, // 播放状态，三种 暂停，播放，结束
      currentTime: '00:00', // 当前播放的位置
      duration: '00:00', // 视频总的时长
      currentPlayPercent: 0, // 当前播放位置占比
    }
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      this.video = this.$refs.videoPlayer;
    },
    canplay() {
      this.showLoading = false;
    },
    statePlay() {
      this.videoState = PLAY;
    },
    statePause() {
      this.videoState = PAUSE;
    },
    stateEnded() {
      this.videoState = ENDED;
    },
    durationchange() {
      this.videoDuration = this.video.duration;
      this.duration = formatVideoTime(this.video.duration);
    },
    // 设置播放进度条
    setProgressBar(witdh) {
      if (this.$refs.currentPlayProgress) {
        this.$refs.currentPlayProgress.style.width = witdh + 'px';
      }
    },
    playVideo() {
      if (this.videoState === PAUSE || this.videoState === ENDED) {
        this.video.play();
      } else {
        this.video.pause();
      }
    },
    timeupdate() {
      // const currentPlayTime = this.video.currentTime;
      this.currentPlayPercent = this.video.currentTime / this.video.duration;
      const currentPlayWidth = (PLAY_TOTAL_WIDTH * this.currentPlayPercent);
      this.currentTime = formatVideoTime(this.video.currentTime);
      this.setProgressBar(currentPlayWidth);
    }
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.progress_bar {
  width: 150px;
  height: 5px;
  /* line-height: 5px; */
  background: darkgrey;
  margin-top: 8px;
  position: relative;
}
.progress {
  width: 0px;
  height: 5px;
  background: pink;
}
.container {
  width: 300px;
  margin: 0 auto;
}
.video_control {
  display: flex;
}
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
