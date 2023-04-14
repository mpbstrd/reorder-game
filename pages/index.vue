<template>
  <div id="app">
    <audio ref="audioPlayer">
      <source :src="audioSrc" type="audio/mpeg">
    </audio>
    <ReorderGame />
  </div>
</template>

<script>
import ReorderGame from "@/components/ReorderGame.vue";
import ScoreStars from "@/components/ScoreStars.vue";

export default {
  name: "App",
  components: {
    ReorderGame,
    ScoreStars,
  },
  data() {
    return {
      audioSrc: '/game.mp3',
    };
  },
  mounted() {
    this.$refs.audioPlayer.addEventListener('canplay', () => {
      this.$refs.audioPlayer.play();
    });
  },
};
</script>
