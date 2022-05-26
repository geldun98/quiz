<template>
  <div class="box-question">
    <div class="box-question-header">
      <div class="box-question-title">
        <h3>Test GMAT (15 phút)</h3>
        <h3>15 câu hỏi trắc nghiệm</h3>
      </div>
      <div class="box-question-logo">
        <img src="@/assets/logo.png" alt="rikkei.edu.vn" />
      </div>
    </div>
    <div class="box-user-info">
      <div class="user-info">
        <div class="user-info-title">Họ và tên</div>
        <div class="user-info-content">{{ user.fullname }}</div>
      </div>
      <div class="user-info">
        <div class="user-info-title">Email</div>
        <div class="user-info-content">{{ user.email }}</div>
      </div>
      <div class="user-info">
        <div class="user-info-title">Phone</div>
        <div class="user-info-content">{{ user.phone }}</div>
      </div>
    </div>
    <div class="box-submit">
      <div class="box-submit-button" @click="handleSubmit()">
        <i class="fa-solid fa-check"></i>
        <span>Nộp bài</span>
      </div>
      <div class="box-timer">
        <i class="fa-solid fa-clock"></i> <span>{{ showTime }}</span>
      </div>
    </div>
    <div
      class="box-question-main"
      v-for="(item, index) in questions"
      :key="index"
    >
      <div class="item-question">
        <h4>{{ item.question }}</h4>
        <div
          class="box-answer"
          v-for="(answer, key, index) in item.answers"
          :key="index"
        >
          <input v-model="responses[item.id]" :value="key" type="radio" />
          <span>{{ answer }}</span>
        </div>
      </div>
    </div>
    <div class="course">
      <h3 class="course-title">Bạn muốn lựa chọn khóa học nào ?</h3>
      <select id="learn-course" v-model="course">
        <option>Lập trình full-stack Java</option>
        <option>Lập trình full-stack Javascript</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: "box-question",
  props: {
    user: Object,
  },
  data() {
    return {
      score: 0,
      totalTime: 900,
      currentTime: null,
      timer: null,
      result: {},
      course: "Lập trình full-stack Java",
      questions: [
        {
          id: "1",
          question:
            "Câu hỏi 1: Một cửa hàng trang trí nội thất chỉ bán hai loại bàn A và B. Giá bàn loại A là 120 đô la, bằng 30% giá bàn loại B. Nếu cửa hàng có 2000 chiếc bàn, trong đó 3/4 là bàn loại B thì tổng số tiền thu được sau khi bán hết số bàn là bao nhiêu? ",
          answers: {
            A: "$114,000",
            B: "$186,000",
            C: "$294,000",
            D: "$660,000",
          },
        },
        {
          id: "2",
          question:
            "Câu hỏi 2: Số cổ phiếu Kathy mua gấp 4 lần số cổ phiếu Carl mua, số cổ phiếu Carl mua gấp 3 lần số cổ phiếu Tom mua. Hỏi số nào dưới đây là tỉ lệ số cổ phiếu mà Kathy mua so với số cổ phiếu mà Tom mua?",
          answers: { A: "3/4", B: "4/3", C: "3/1", D: "12/1" },
        },
        {
          id: "3",
          question: "Câu hỏi 3: 3,003/2,002=",
          answers: { A: "1,05", B: "1,50015", C: "1,501", D: "1,5" },
        },
        {
          id: "4",
          question:
            "Câu hỏi 4: Năm ngoái, cứ 100 triệu xe chạy trên một con đường nào đó sẽ có 96 xe gây tai nạn.Nếu có 3 tỷ xe chạy trên đường thì có bao nhiêu xe gây tai nạn? (1 tỷ = 1.000.000.000).",
          answers: { A: "288", B: "320", C: "2.880", D: "3.200" },
        },
        {
          id: "5",
          question: "Câu hỏi 5: 45% của 7/12 của 240 có giá trị là bao nhiêu?",
          answers: { A: "63", B: "90", C: "108", D: "140" },
        },
        {
          id: "6",
          question: "Câu hỏi 6: Nếu 45% của n là 405 thì 35% của n là?",
          answers: { A: "61", B: "64", C: "142", D: "315" },
        },
        {
          id: "7",
          question:
            "Câu hỏi 7: Chiếc đồng hồ nhắc việc cứ 15 phút lại reo 1 lần. Nếu đồng hồ đã reo vào lúc 12:40 thì thời điểm nào dưới đây là thời điểm mà đồng hồ có thể reo?",
          answers: { A: "4h05p", B: "5h30p", C: "6h45p", D: "8h10p" },
        },
        {
          id: "8",
          question:
            "Câu hỏi 8: Nếu x ≥8 và y ≤3 thì điều nào sau đây chắc chắn đúng?",
          answers: {
            A: "x + y ≥ 5",
            B: "x + y ≤ 1",
            C: "x – y ≥ 5",
            D: "x – y ≤ 5",
          },
        },
        {
          id: "9",
          question:
            "Câu hỏi 9: Trung bình của 6, 8 và 10 bằng trung bình của 7, 9, và",
          answers: { A: "5", B: "7", C: "8", D: "9" },
        },
        {
          id: "10",
          question:
            "Câu hỏi 10: Nhà Thanh có ao bèo 1.000m2, ngày hôm sau số lượng bèo sẽ nở gấp đôi ngày hôm trước, đến ngày thứ 18 thì bèo đã nở được nửa ao. Vậy đến ngày thứ bao nhiêu thì bèo sẽ nở đầy ao?",
          answers: {
            A: "Ngày thứ 1",
            B: "Ngày thứ 19",
            C: "Ngày thứ 36",
            D: "Ngày thứ 42",
          },
        },
        {
          id: "11",
          question:
            "Câu hỏi 11: 6 con cáo ăn thịt 6 con gà trong vòng 6 phút. Vậy phải bao nhiêu con cáo mới có thể ăn hết 60 con gà trong vòng 1 giờ?",
          answers: { A: "6", B: "12", C: "30", D: "60" },
        },
        {
          id: "12",
          question:
            "Câu hỏi 12: Nếu a = b , b > c và c = d thì mối quan hệ giữa a và d là?",
          answers: { A: "a>d", B: "a<d", C: "a=d", D: "Không xác định được" },
        },
        {
          id: "13",
          question:
            "Câu hỏi 13: Nếu từ WOLF tương ứng với số 8526, thì từ FLOW tương ứng với số nào sau đây?",
          answers: { A: "6258", B: "2856", C: "5862", D: "6852" },
        },
        {
          id: "14",
          question:
            "Câu 14: Nếu 5y – 3x = 7 và 6y + 6x = 2 thì giá trị của y là?",
          answers: { A: "1", B: "2", C: "3", D: "4" },
        },
        {
          id: "15",
          question:
            "Câu 15: Một vài fan của Trấn Thành là fan của Trường Giang. Và một vài fan của Trường Giang đồng thời là fan của Lê Giang. Như vậy một vài fan của Trấn Thành chắc chắn là fan của Lê Giang?",
          answers: { A: "Rất chuẩn", B: "Sai rồi" },
        },
      ],
      responses: {},
      corrects: {
        1: "D",
        2: "D",
        3: "D",
        4: "C",
        5: "A",
        6: "D",
        7: "D",
        8: "C",
        9: "C",
        10: "B",
        11: "A",
        12: "A",
        13: "A",
        14: "A",
        15: "B",
      },
    };
  },
  methods: {
    async handleSubmit() {
      for (let i = 1; i <= 15; i++) {
        if (this.corrects[i] === this.responses[i]) {
          this.score++;
        }
      }
      this.result = {
        score: `${this.score}/${this.questions.length}`,
        time: this.calculateTime(this.totalTime - this.currentTime),
      };
      clearInterval(this.timer);
      await this.sendGoogleSheet();

      this.$emit("handleQuestion", this.result);
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
      formData.append("Fullname", this.user.fullname);
      formData.append("Email", this.user.email);
      formData.append("Phone", this.user.phone);
      formData.append("Point", this.result.score);
      formData.append("Course", this.course);

      formData.append("1", this.responses[1]);
      formData.append("2", this.responses[2]);
      formData.append("3", this.responses[3]);
      formData.append("4", this.responses[4]);
      formData.append("5", this.responses[5]);
      formData.append("6", this.responses[6]);
      formData.append("7", this.responses[7]);
      formData.append("8", this.responses[8]);
      formData.append("9", this.responses[9]);
      formData.append("10", this.responses[10]);
      formData.append("11", this.responses[11]);
      formData.append("12", this.responses[12]);
      formData.append("13", this.responses[13]);
      formData.append("14", this.responses[14]);
      formData.append("15", this.responses[15]);

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
        clearInterval(this.timer);
        this.handleSubmit();
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.box-question {
  max-width: 1120px;
  min-height: 200vh;

  margin: 0 auto;
  &-title {
    h3 {
      font-weight: 500;
      margin-bottom: 10px;
    }
  }
  &-header {
    position: relative;
    padding-top: 10px;
    border-bottom: 2px solid #bdc3c7;
  }
  &-logo {
    position: absolute;

    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    img {
      height: 30px;
    }
  }
  .box-user-info {
    padding-top: 15px;
  }
  .user-info {
    display: flex;
    &-title {
      width: 100px;
      font-weight: 600;
      margin-bottom: 10px;
    }
  }

  .box-submit-button {
    i {
      color: white;
    }
    background: #e74c3c;
    display: inline-block;
    padding: 6px 12px;
    border-radius: 5px;
    cursor: pointer;
    span {
      color: white;
      margin-left: 5px;
    }
  }
  .box-timer {
    i {
      color: white;
    }
    span {
      color: white;
      margin-left: 5px;
    }
    background: #3498db;
    display: inline-block;
    padding: 6px 12px;
    border-radius: 5px;
    margin-left: 10px;
  }
  .box-submit {
    position: fixed;

    top: 10px;
    right: calc((100% - 1120px) / 2);
    display: inline-block;
    border: 2px solid #2c3e50;
    padding: 7px;
  }
  .item-question {
    background: #ecf0f1;
    padding: 15px 10px;
    margin-bottom: 15px;
    h4 {
      font-weight: 500;
      margin-bottom: 10px;
    }
  }
  .box-answer {
    input {
      margin-bottom: 10px;
    }
    span {
      margin-left: 5px;
    }
  }
  .course {
    background: #ecf0f1;
    padding: 10px;
    padding-bottom: 20px;
    margin-bottom: 100px;
    &-title {
      font-size: 16px;
    }
    select {
      margin-top: 10px;
      font-family: "Roboto", sans-serif;
      font-size: 16px;
      outline: none;
      padding: 5px 10px;
    }
  }
}
</style>
