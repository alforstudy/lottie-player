<template>
  <div id="app">
    <lottie-player />
  </div>
</template>

<script>
import LottiePlayer from "@src/components/lottie-player";

export default {
  name: "app",
  components: {
    LottiePlayer
  },
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  }
};
</script>

<style>
</style>
