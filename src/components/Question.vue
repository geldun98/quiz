<template>
  <div class="question">
    <div class="quiz-time">
      <img src="@/assets/clock.png" alt="clock" />
      {{ showTime }}
    </div>
    <div
      class="quiz-main"
      v-for="(element, index) in questions.slice(a, b)"
      :key="index"
    >
      <div class="box-question">
        <h2>Câu {{ b }}: {{ element.question }}</h2>
      </div>

      <ul class="box-answer" :class="isClick ? '' : 'preventClick'">
        <li
          v-for="(item, index) in element.answers"
          :key="index"
          :class="select ? check(item) : ''"
          @click="selectResponse(item)"
        >
          {{ item.answer }}
        </li>
      </ul>
    </div>
    <div class="quiz-footer">
      <div class="box-button">
        <button
          :class="isClick ? 'preventClick hideButton' : ''"
          @click="handleNext"
        >
          Tiếp tục
        </button>
      </div>
    </div>
    <div v-if="isLoading" class="quiz-loading">
      Đang gửi dữ liệu, bạn vui lòng !
    </div>
  </div>
</template>

<script>
export default {
  name: "run-comp",
  props: {
    user: Object,
  },
  data() {
    return {
      totalTime: 1000,
      currentTime: null,
      timer: null,
      isLoading: false,
      a: 0,
      b: 1,
      score: 0,
      select: false,
      isClick: true,
      result: null,
      questions: [
        {
          question: "5-2",
          answers: [
            { answer: "3", correct: true },
            { answer: "4" },
            { answer: "5" },
            { answer: "6" },
          ],
        },
        {
          question: "1+1",
          answers: [
            { answer: "1" },
            { answer: "2", correct: true },
            { answer: "3" },
            { answer: "4" },
          ],
        },
        {
          question: "2*2",
          answers: [
            { answer: "2" },
            { answer: "3" },
            { answer: "5" },
            { answer: "4", correct: true },
          ],
        },
      ],
    };
  },
  methods: {
    handleNext() {
      this.a = this.a + 1;
      this.b = this.b + 1;
      this.select = false;
      this.isClick = true;
      if (this.a === this.questions.length) {
        this.handleSubmit();
      }
    },
    async handleSubmit() {
      clearInterval(this.timer);
      this.result = {
        score: `${this.score}/${this.questions.length}`,
        time: this.calculateTime(this.totalTime - this.currentTime),
      };
      this.isLoading = true;
      await this.sendGoogleSheet();
      this.isLoading = false;
      this.$emit("handleQuestion", this.result);
    },
    selectResponse(e) {
      this.select = true;
      this.isClick = false;
      if (e.correct) {
        this.score = this.score + 1;
      }
    },
    check(status) {
      if (status.correct) {
        return "correct";
      } else {
        return "incorrect";
      }
    },
    updateTime() {
      this.currentTime = this.currentTime - 1;
    },
    calculateTime(total) {
      let minutes = Math.floor(total / 60);
      let seconds = total % 60;
      if (minutes < 10) {
        minutes = `0${minutes}`;
      }
      if (seconds < 10) {
        seconds = `0${seconds}`;
      }
      return `${minutes}:${seconds}`;
    },
    async sendGoogleSheet() {
      const scriptURL =
        "https://script.google.com/macros/s/AKfycbzpk0GTB6YkB2C7cDQTphCIyj2zWJtLiZwkALYjyO44gmqnvRO4TNLoNFIpepm3C8J4/exec";

      let formData = new FormData();
      formData.append("Fullname", this.user.name);
      formData.append("Email", this.user.email);
      formData.append("Phone", this.user.phone);
      formData.append("Point", this.result.score);
      await fetch(scriptURL, { method: "POST", body: formData });
    },
  },
  mounted() {
    this.currentTime = this.totalTime;
    this.timer = setInterval(this.updateTime, 1000);
  },
  computed: {
    showTime() {
      return this.calculateTime(this.currentTime);
    },
  },
  watch: {
    currentTime() {
      if (this.currentTime < 1) {
        this.handleSubmit();
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@mixin maxWidth($width) {
  @media screen and (max-width: $width) {
    @content;
  }
}
@mixin minWidth($width) {
  @media screen and (min-width: $width) {
    @content;
  }
}
.quiz-loading {
  font-size: 20px;
  font-weight: 600;
  text-align: center;
}
.question {
  max-width: 800px;
  margin: 0 auto;
  background: white;
  padding: 25px;
  position: relative;
  top: 100px;
  border-radius: 10px;
}
.quiz-main {
  margin: 0 auto;
}
.box-answer {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;

  li {
    width: 45%;
    font-size: 18px;
    padding: 10px;
    border-radius: 10px;
  }
}
.box-button {
  margin-top: 10px;
  text-align: center;
  button {
    background: #e74c3c;
    color: white;
    font-size: 18px;
    outline: none;
    border: 0;
    padding: 10px 15px;
    border-radius: 10px;
    cursor: pointer;
  }
}

li {
  list-style: none;
  background: #7f8c8d;
  color: white;
  margin: 15px 0;
  outline: 2px solid transparent;
  cursor: pointer;
  &:hover {
    outline-color: #2c3e50;
  }
  &.correct {
    background-color: #3498db;
    color: white;
    font-weight: 600;
  }
  &.incorrect {
    background-color: #e74c3c;
    color: white;
    font-weight: 600;
  }
}
.preventClick {
  pointer-events: none !important;
}
.hideButton {
  background: transparent !important;
  color: transparent !important;
}
.quiz-time {
  position: absolute;
  display: flex;
  align-items: center;
  right: 0;
  top: -30px;
  font-size: 20px;
  img {
    height: 20px;
    margin-right: 5px;
  }
}
@include maxWidth(767px) {
  .box-answer {
    flex-direction: column;
    li {
      width: 100%;
      font-size: 18px;
      padding: 10px;
      border-radius: 10px;
    }
  }
}
</style>
