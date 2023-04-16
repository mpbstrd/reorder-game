<template>
  <div id="app">
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
};
</script>
