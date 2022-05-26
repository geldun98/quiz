<template>
  <div class="result">
    <div class="banner">
      <img src="@/assets/banner.png" alt="banner" />
    </div>
    <div class="result-detail">
      <h2>Thí sinh: <slot name="user"> </slot></h2>
      <p>Điểm số: <slot name="result-score"></slot></p>
      <p>Thời gian làm bài: <slot name="result-time"></slot></p>
    </div>
  </div>
</template>

<script>
export default {
  name: "end-comp",
};
</script>

<style lang="scss" scoped>
.result {
  margin: 0 auto;
  background: white;
  width: 100%;
  max-width: 600px;
  padding: 25px;
  border-radius: 10px;
  position: relative;
  top: 150px;
  .banner {
    width: 100%;
    display: flex;
    justify-content: right;
    img {
      width: 150px;
    }
  }
}
</style>
