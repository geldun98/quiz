<template>
  <div id="app">
    <Register
      v-if="isShow.register"
      @handleRegister="handleRegister"
    ></Register>
    <Question
      v-if="isShow.question"
      @handleQuestion="handleQuestion"
      :user="user"
    >
    </Question>
    <Result v-if="isShow.result">
      <template slot="user">{{ user.name }}</template>
      <template slot="result-score">{{ result.score }}</template>
      <template slot="result-time">{{ result.time }}</template>
    </Result>
  </div>
</template>

<script>
import Result from "./components/Result.vue";
import Question from "./components/Question.vue";
import Register from "./components/Register.vue";
export default {
  name: "App",
  components: { Register, Question, Result },
  data() {
    return {
      isShow: {
        register: true,
        question: false,
        result: false,
      },
      user: {
        name: "",
        email: "",
      },
      result: {
        time: "0",
        score: 10,
      },
    };
  },
  methods: {
    handleRegister(dataUser) {
      this.user = { ...dataUser };
      this.isShow.register = false;
      this.isShow.question = true;
    },
    handleQuestion(dataResult) {
      this.isShow.question = false;
      this.isShow.result = true;
      this.result = { ...dataResult };
    },
  },
};
</script>

<style lang="scss" scoped>
#app {
  width: 100%;
  min-height: 100vh;
  position: relative;
  background-image: url("@/assets/background.png");
  background-color: #ecf0f1;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
