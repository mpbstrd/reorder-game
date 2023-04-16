<template>
  <div>
    <span v-for="(star, index) in stars" :key="index" :class="{ filled: index < score }" style="font-size: 5em;">&#9733;</span>
  </div>
</template>


<script>
  export default {
    props: {
      score: {
        type: Number,
        required: true,
        validator: (value) => value >= 0 && value <= 5,
      },
    },
    computed: {
      stars() {
        return [1, 2, 3, 4, 5];
      },
    },
  };
</script>
  
  
  <style scoped>
  .filled {
    color: orange;
  }
  </style>
  