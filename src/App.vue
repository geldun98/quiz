<template>
  <div id="app">
    <Register
      v-if="isShow.register"
      @handleRegister="handleRegister"
    ></Register>
    <BoxQuestion
      v-if="isShow.boxquestion"
      :user="user"
      @handleQuestion="handleQuestion"
    ></BoxQuestion>
    <End :user="user"></End>
  </div>
</template>

<script>
import BoxQuestion from "./components/BoxQuestion.vue";
import End from "./components/End.vue";
import Register from "./components/Register.vue";
export default {
  name: "App",
  components: { Register, BoxQuestion, End },

  data() {
    return {
      isShow: {
        register: true,
        boxquestion: false,
        end: false,
      },
      user: {},
    };
  },
  methods: {
    handleRegister(dataUser) {
      this.user = { ...dataUser };
      this.isShow.register = false;
      this.isShow.boxquestion = true;
    },
    handleQuestion(result) {
      this.user = { ...this.user, ...result };
      this.isShow.boxquestion = false;
      this.isShow.end = true;
    },
  },
};
</script>

<style lang="scss">
#app {
}
</style>
