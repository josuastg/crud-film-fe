<template>
  <div class="wrapper-card">
    <div class="shimmerBG media"></div>
  </div>
</template>

<script>
export default {
  name: "ShimmerCard",
  props: {},
};
</script>

<style scoped>
.wrapper-card {
  display: flex;
  flex-direction: column;
  width: 16em;
  height: 20em;
  margin-bottom: 60px;
}
.shimmerBG {
  animation-duration: 2.2s;
  animation-fill-mode: forwards;
  animation-iteration-count: infinite;
  animation-name: shimmer;
  animation-timing-function: linear;
  background: #ddd;
  background: linear-gradient(to right, #f6f6f6 8%, #f0f0f0 18%, #f6f6f6 33%);
  background-size: 1200px 100%;
}
.media {
  width: 16em;
  height: 20em;
  border-radius: 5px;
}

@-webkit-keyframes shimmer {
  0% {
    background-position: -100% 0;
  }
  100% {
    background-position: 100% 0;
  }
}

@keyframes shimmer {
  0% {
    background-position: -1200px 0;
  }
  100% {
    background-position: 1200px 0;
  }
}
</style>
