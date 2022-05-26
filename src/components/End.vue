<template>
  <div class="end">
    <div class="end-form">
      <div class="end-logo">
        <img src="@/assets/logo.png" alt="rikkei.edu.vn" />
      </div>
      <h3>Kết quả bài thi</h3>
      <div class="user-info">
        <div class="user-info-title">Họ và tên:</div>
        <div class="user-info-content">{{ user.fullname }}</div>
      </div>
      <div class="user-info">
        <div class="user-info-title">Số câu trả lời đúng:</div>
        <div class="user-info-content">{{ user.score }}</div>
      </div>
      <div class="user-info">
        <div class="user-info-title">Thời gian làm bài:</div>
        <div class="user-info-content">{{ user.time }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "end-comp",
  props: {
    user: Object,
  },
};
</script>

<style lang="scss" scoped>
.end {
  width: 100%;
  min-height: 100vh;
  background-image: url("@/assets/background.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  &-form {
    width: 100%;
    max-width: 540px;
    background: #ecf0f1;
    padding: 25px;

    border-radius: 10px;
    h3 {
      text-align: center;
      margin-bottom: 15px;
    }
  }
  &-logo {
    text-align: center;
    padding: 15px 0;
    img {
      height: 40px;
    }
  }
  .user-info {
    display: flex;
    &-title {
      font-weight: 500;
      width: 150px;
    }
    margin-bottom: 15px;
  }
}
</style>
