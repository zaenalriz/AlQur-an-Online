<template>
  <div
    class="w-full bg-gray-100 p-3 flex justify-between lg:px-36 fixed bottom-0 z-0 py-4"
  >
    <div class="flex items-center">
      <!-- <div v-if="Audio">play</div>
      <div v-else>
      </div> -->
      <icon-pause></icon-pause>
      <div class="relative w-96">
        <div class="h-3 rounded-2xl bg-gray-300 w-full"></div>
        <div
          class="h-3 rounded-2xl bg-green-500 progress absolute top-0"
          ref="progress"
        ></div>
      </div>
    </div>
    <div v-if="play">
      {{ Play() }}
    </div>
  </div>
</template>
<script>
import pause from "@/components/icons/pause.vue";
export default {
  components: {
    "icon-pause": pause,
  },
  props: {
    apiaudio: {
      type: Array,
      required: true,
    },
    play: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      Audio: this.$props.play,
    };
  },
  methods: {
    Play() {
      let audio = this.$props.apiaudio[0];
      this.$store.state.play = true;
      if (audio) {
        const playAudio = new Audio(audio);
        playAudio.play();
        playAudio.addEventListener("timeupdate", this.progress);
      }
    },
    progress(e) {
      const { duration, currentTime } = e.srcElement;
      const progress = (currentTime / duration) * 100;
      const styleProgress = this.$refs.progress;
      styleProgress.style.width = `${progress}%`;
      if (progress === 100) {
        this.$store.state.play = false;
      }
    },
  },
};
</script>
<style>
.progress {
  width: 0%;
}
</style>
