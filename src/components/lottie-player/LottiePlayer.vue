<template>
  <div id="player" ref="player">
    <div class="control">
      <div class="control-mask"></div>
      <div class="progress-bar"></div>
      <div class="current-progress-bar" :style="{width:progressBarWidth}"></div>
      <div class="play-pause">
        <img v-if="!playStatus || endStatus" :src="playSvg" @click="playLottie" />
        <img v-else :src="pauseSvg" @click="pauseLottie" />
        <div class="times">{{currentAndallTimes}}</div>
      </div>
    </div>
  </div>
</template>

<script>
import lottie from "lottie-web";
import { adrock, bodymovin } from "@src/consts/mock";
import playSvg from "@src/assets/play.svg";
import pauseSvg from "@src/assets/pause.svg";

export default {
  name: "lottie-player",
  data() {
    return {
      msg: "Hello World",
      playSvg,
      pauseSvg,
      allTimes: 0,
      currentFrame: 0,
      allFrames: 0,
      fr: 0,
      playStatus: false,
      endStatus: false,
      lottie: null
    };
  },
  computed: {
    currentAndallTimes() {
      return `${this.currentTimes}/${this.allTimes.toFixed(0)}`;
    },
    currentTimes() {
      return (this.currentFrame / this.fr).toFixed(0);
    },
    progressBarWidth() {
      const width = ((this.currentFrame * 100) / this.allFrames).toFixed(2);
      if (width && Number(width)) {
        return width + "%";
      } else {
        return "1%";
      }
    }
  },
  mounted() {
    this.lottie = lottie.loadAnimation({
      container: this.$refs["player"],
      renderer: "svg",
      loop: false,
      autoplay: false,
      animationData: adrock
    });
    this.animationData = adrock;
    this.fr = this.animationData.fr;
    this.allTimes = this.lottie.getDuration();
    this.allFrames = this.lottie.getDuration(true);
    this.lottie.addEventListener("enterFrame", r => {
      this.currentFrame = r.currentTime.toFixed(0);
    });
    this.lottie.addEventListener("complete", r => {
      this.endStatus = true;
    });
  },
  methods: {
    playLottie() {
      if (this.endStatus) {
        this.lottie.goToAndStop(0);
        this.endStatus = false;
      }
      this.lottie.play();
      this.playStatus = true;
    },
    pauseLottie() {
      this.lottie.pause();
      this.playStatus = false;
    }
  }
};
</script>

<style scoped>
#player {
  width: 300px;
  height: 533px;
  position: relative;
  background-color: #000;
}

.control {
  z-index: 1;
  position: absolute;
  bottom: 0;
  background-color: #000;
  opacity: 0.55;
  height: 44px;
  width: 100%;
}

.control-mask {
  position: absolute;
  bottom: 0;
  left: 0;
  background-position: bottom;
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAADGCAYAAAAT+OqFAAAAdklEQVQoz42QQQ7AIAgEF/T/D+kbq/RWAlnQyyazA4aoAB4FsBSA/bFjuF1EOL7VbrIrBuusmrt4ZZORfb6ehbWdnRHEIiITaEUKa5EJqUakRSaEYBJSCY2dEstQY7AuxahwXFrvZmWl2rh4JZ07z9dLtesfNj5q0FU3A5ObbwAAAABJRU5ErkJggg==)
    repeat-x bottom;
  transition: all 0.2s ease-in-out;
  z-index: 50;
  opacity: 0;
  width: 100%;
  height: 100px;
  pointer-events: none;
}

.control .progress-bar {
  position: absolute;
  top: 0;
  background-color: hsla(0, 0%, 100%, 0.2);
  height: 4px;
  width: 100%;
  border-radius: 1px;
}

.control .current-progress-bar {
  position: absolute;
  top: 0;
  background-color: rgba(236, 119, 10, 0.952);
  height: 4px;
  width: 20%;
  border-radius: 1px;
}

.control .current-progress-bar:hover {
  transform: scaleY(1.2);
}
.control .progress-bar:hover {
  transform: scaleY(1.2);
}

.control .play-pause {
  height: 100%;
  display: flex;
  align-items: center;
  margin-left: 10px;
}

.control .play-pause img {
  width: 20px;
  height: 20px;
}

.control .play-pause .times {
  font-size: 14px;
  margin-left: 12px;
}
</style>