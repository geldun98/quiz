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
        <h2>{{ element.question }}</h2>
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
      totalTime: 1200,
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
          question:
            "Câu hỏi 1: Một cửa hàng trang trí nội thất chỉ bán hai loại bàn A và B. Giá bàn loại A là 120 đô la, bằng 30% giá bàn loại B. Nếu cửa hàng có 2000 chiếc bàn, trong đó 3/4 là bàn loại B thì tổng số tiền thu được sau khi bán hết số bàn là bao nhiêu? ",
          answers: [
            { answer: "A: $114,000", position: "A" },
            { answer: "B: $186,000", position: "B" },
            { answer: "C: $294,000", position: "C" },
            { answer: "D: $660,000", position: "D", correct: true },
          ],
        },
        {
          question:
            "Câu hỏi 2: Số cổ phiếu Kathy mua gấp 4 lần số cổ phiếu Carl mua, số cổ phiếu Carl mua gấp 3 lần số cổ phiếu Tom mua. Hỏi số nào dưới đây là tỉ lệ số cổ phiếu mà Kathy mua so với số cổ phiếu mà Tom mua?",
          answers: [
            { answer: "A:	3/4", position: "A" },
            { answer: "B:	4/3", position: "B" },
            { answer: "C:	3/1", position: "C" },
            { answer: "D:	12/1", position: "D", correct: true },
          ],
        },
        {
          question: "Câu hỏi 3: 3,003/2,002=",
          answers: [
            { answer: "A: 1,05", position: "A" },
            { answer: "B: 1,50015", position: "B" },
            { answer: "C: 1,501", position: "C" },
            { answer: "D: 1,5", position: "D", correct: true },
          ],
        },
        {
          question:
            "Câu hỏi 4: Năm ngoái, cứ 100 triệu xe chạy trên một con đường nào đó sẽ có 96 xe gây tai nạn.Nếu có 3 tỷ xe chạy trên đường thì có bao nhiêu xe gây tai nạn? (1 tỷ = 1.000.000.000).",
          answers: [
            { answer: "A: 288", position: "A" },
            { answer: "B: 320", position: "B" },
            { answer: "C: 2.880 ", position: "C", correct: true },
            { answer: "D: 3.200", position: "D" },
          ],
        },
        {
          question: "Câu hỏi 5: 45% của 7/12 của 240 có giá trị là bao nhiêu?",
          answers: [
            { answer: "A: 63", position: "A", correct: true },
            { answer: "B: 90", position: "B" },
            { answer: "C: 108", position: "C" },
            { answer: "D: 140", position: "D" },
          ],
        },
        {
          question: "Câu hỏi 6: Nếu 45% của n là 405 thì 35% của n là?",
          answers: [
            { answer: "A: 61", position: "A" },
            { answer: "B: 64", position: "B" },
            { answer: "C: 142", position: "C" },
            { answer: "D: 315", position: "D", correct: true },
          ],
        },
        {
          question:
            "Câu hỏi 7: Chiếc đồng hồ nhắc việc cứ 15 phút lại reo 1 lần. Nếu đồng hồ đã reo vào lúc 12:40 thì thời điểm nào dưới đây là thời điểm mà đồng hồ có thể reo?",
          answers: [
            { answer: "A: 4h05p", position: "A" },
            { answer: "B: 5h30p", position: "B" },
            { answer: "C: 6h45p", position: "C" },
            { answer: "D: 8h10p", position: "D", correct: true },
          ],
        },
        {
          question:
            "Câu hỏi 8: Nếu x ≥8 và y ≤3 thì điều nào sau đây chắc chắn đúng?",
          answers: [
            { answer: "A: x + y ≥5", position: "A" },
            { answer: "B: x + y ≤11", position: "B" },
            { answer: "C: x – y ≥5", position: "C", correct: true },
            { answer: "D: x – y ≤5", position: "D" },
          ],
        },
        {
          question:
            "Câu hỏi 9: Trung bình của 6, 8 và 10 bằng trung bình của 7, 9, và",
          answers: [
            { answer: "A: 5", position: "A" },
            { answer: "B: 7", position: "B" },
            { answer: "C: 8", position: "C", correct: true },
            { answer: "D: 9", position: "D" },
          ],
        },
        {
          question:
            "Câu hỏi 10: Nhà Thanh có ao bèo 1.000m2, ngày hôm sau số lượng bèo sẽ nở gấp đôi ngày hôm trước, đến ngày thứ 18 thì bèo đã nở được nửa ao. Vậy đến ngày thứ bao nhiêu thì bèo sẽ nở đầy ao?",
          answers: [
            { answer: "A: Ngày thứ 1", position: "A" },
            { answer: "B: Ngày thứ 19", position: "B", correct: true },
            { answer: "C: Ngày thứ 36", position: "C" },
            { answer: "D: Ngày thứ 42", position: "D" },
          ],
        },
        {
          question:
            "Câu hỏi 11: 6 con cáo ăn thịt 6 con gà trong vòng 6 phút. Vậy phải bao nhiêu con cáo mới có thể ăn hết 60 con gà trong vòng 1 giờ?",
          answers: [
            { answer: "A: 6", position: "A", correct: true },
            { answer: "B: 12", position: "B" },
            { answer: "C: 30", position: "C" },
            { answer: "D: 60", position: "D" },
          ],
        },
        {
          question:
            "Câu hỏi 12: Nếu a = b , b > c và c = d thì mối quan hệ giữa a và d là?",
          answers: [
            { answer: "A: a>d", position: "A", correct: true },
            { answer: "B: a<d", position: "B" },
            { answer: "C: a=d", position: "C" },
            { answer: "D: Không xác định được", position: "D" },
          ],
        },
        {
          question:
            "Câu hỏi 13: Nếu từ WOLF tương ứng với số 8526, thì từ FLOW tương ứng với số nào sau đây?",
          answers: [
            { answer: "A: 6258", position: "A", correct: true },
            { answer: "B: 2856", position: "B" },
            { answer: "C: 5862", position: "C" },
            { answer: "D: 6852", position: "D" },
          ],
        },
        {
          question:
            "Câu 14: Nếu 5y – 3x = 7 và 6y + 6x = 2 thì giá trị của y là?",
          answers: [
            { answer: "A: 1", position: "A", correct: true },
            { answer: "B: 2", position: "B" },
            { answer: "C: 3", position: "C" },
            { answer: "D: 4", position: "D" },
          ],
        },
        {
          question:
            "Câu 15: Một vài fan của Trấn Thành là fan của Trường Giang. Và một vài fan của Trường Giang đồng thời là fan của Lê Giang. Như vậy một vài fan của Trấn Thành chắc chắn là fan của Lê Giang?",
          answers: [
            { answer: "A: Rất chuẩn", position: "A" },
            { answer: "B: Sai rồi", position: "B", correct: true },
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
