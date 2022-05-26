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
    <div v-if="isCourse" class="course">
      <h2 class="course-title">Bạn muốn lựa chọn khóa học nào ?</h2>
      <select id="learn-course" v-model="course">
        <option>Lập trình Web full-stack Java</option>
        <option>Lập trình Web full-stack Javascript</option>
      </select>
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
      arrAnswer: [],
      totalTime: 900,
      currentTime: null,
      timer: null,
      isLoading: false,
      a: 0,
      b: 1,
      score: 0,
      select: false,
      isClick: true,
      result: null,
      isCourse: false,
      course: "Lập trình Web full-stack Java",
      questions: [
        {
          question: "5-2",
          answers: [
            { answer: "3", position: "A", correct: true },
            { answer: "4", position: "B" },
            { answer: "5", position: "C" },
            { answer: "6", position: "D" },
          ],
        },
        {
          question: "1+1",
          answers: [
            { answer: "1", position: "A" },
            { answer: "2", position: "B", correct: true },
            { answer: "3", position: "C" },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "2*2",
          answers: [
            { answer: "2", position: "A" },
            { answer: "3", position: "B" },
            { answer: "5", position: "C" },
            { answer: "4", position: "D", correct: true },
          ],
        },
        {
          question: "2*3",
          answers: [
            { answer: "2", position: "A" },
            { answer: "3", position: "B" },
            { answer: "5", position: "C" },
            { answer: "6", position: "D", correct: true },
          ],
        },
        {
          question: "5+3",
          answers: [
            { answer: "2", position: "A" },
            { answer: "3", position: "B" },
            { answer: "5", position: "C" },
            { answer: "8", position: "D", correct: true },
          ],
        },
        {
          question: "3-3",
          answers: [
            { answer: "0", position: "A", correct: true },
            { answer: "3", position: "B" },
            { answer: "5", position: "C" },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "3*2",
          answers: [
            { answer: "2", position: "A" },
            { answer: "3", position: "B" },
            { answer: "6", position: "C", correct: true },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "0*2",
          answers: [
            { answer: "2", position: "A" },
            { answer: "3", position: "B" },
            { answer: "0", position: "C", correct: true },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "9*2",
          answers: [
            { answer: "2", position: "A" },
            { answer: "3", position: "B" },
            { answer: "18", position: "C", correct: true },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "7+5",
          answers: [
            { answer: "2", position: "A" },
            { answer: "16", position: "B" },
            { answer: "12", position: "C", correct: true },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "6*9",
          answers: [
            { answer: "54", position: "A", correct: true },
            { answer: "16", position: "B" },
            { answer: "0", position: "C" },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "1+8",
          answers: [
            { answer: "2", position: "A" },
            { answer: "16", position: "B" },
            { answer: "0", position: "C" },
            { answer: "9", position: "D", correct: true },
          ],
        },
        {
          question: "4*4",
          answers: [
            { answer: "2", position: "A" },
            { answer: "16", position: "B", correct: true },
            { answer: "0", position: "C" },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "8*6",
          answers: [
            { answer: "2", position: "A" },
            { answer: "16", position: "B" },
            { answer: "48", position: "C", correct: true },
            { answer: "4", position: "D" },
          ],
        },
        {
          question: "7*7",
          answers: [
            { answer: "49", position: "A", correct: true },
            { answer: "16", position: "B" },
            { answer: "0", position: "C" },
            { answer: "4", position: "D" },
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
        clearInterval(this.timer);
        this.result = {
          score: `${this.score}/${this.questions.length}`,
          time: this.calculateTime(this.totalTime - this.currentTime),
        };
        this.isCourse = true;
        this.isClick = false;
      }
      if (this.a > this.questions.length) {
        this.handleSubmit();
      }
    },
    async handleSubmit() {
      await this.sendGoogleSheet();
      this.$emit("handleQuestion", this.result);
    },
    selectResponse(e) {
      this.select = true;
      this.isClick = false;
      if (e.correct) {
        this.score = this.score + 1;
      }
      e.active = true;
      this.arrAnswer.push(e.position);
    },
    check(status) {
      if (status.correct) {
        return "correct";
      }
      if (status.active && !status.correct) {
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
      formData.append("Course", this.course);

      formData.append("1", this.arrAnswer[0]);
      formData.append("2", this.arrAnswer[1]);
      formData.append("3", this.arrAnswer[2]);
      formData.append("4", this.arrAnswer[3]);
      formData.append("5", this.arrAnswer[4]);
      formData.append("6", this.arrAnswer[5]);
      formData.append("7", this.arrAnswer[6]);
      formData.append("8", this.arrAnswer[7]);
      formData.append("9", this.arrAnswer[8]);
      formData.append("10", this.arrAnswer[9]);
      formData.append("11", this.arrAnswer[10]);
      formData.append("12", this.arrAnswer[11]);
      formData.append("13", this.arrAnswer[12]);
      formData.append("14", this.arrAnswer[13]);
      formData.append("15", this.arrAnswer[14]);
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
select {
  margin-top: 15px;
  font-family: "Source Sans Pro", sans-serif;
  font-size: 18px;
  outline: none;
  padding: 5px 10px;
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
