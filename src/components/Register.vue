<template>
  <div class="register">
    <div class="register-form">
      <div class="banner">
        <img src="@/assets/banner.png" alt="banner" />
      </div>
      <div class="input-filed input-name">
        <input
          v-model="name"
          type="text"
          placeholder="Nhập tên"
          :class="{ validateFiled: error.name }"
          @focus="handleFocus('name')"
        />
        <div v-if="error.name" class="show-message">
          Vui lòng nhập đầy đủ tên
        </div>
      </div>
      <div class="input-filed input-email">
        <input
          v-model="email"
          type="text"
          placeholder="Nhập email"
          @focus="handleFocus('email')"
          :class="{ validateFiled: error.email }"
        />
        <div v-if="error.email" class="show-message" :class="error">
          Vui lòng nhập email
        </div>
      </div>
      <div class="input-filed input-phone">
        <input
          v-model="phone"
          type="text"
          placeholder="Nhập số điện thoại"
          @focus="handleFocus('phone')"
          :class="{ validateFiled: error.phone }"
        />
        <div v-if="error.phone" class="show-message" :class="error">
          Vui lòng nhập số điện thoại
        </div>
      </div>
      <div class="register-submit">
        <button @click="handleSubmit">Bắt đầu làm bài</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "start-comp",
  data() {
    return {
      name: "",
      email: "",
      phone: "",
      error: {
        name: false,
        email: false,
        phone: false,
      },
    };
  },
  methods: {
    handleSubmit() {
      if (this.name.trim().length < 2) {
        this.error.name = true;
      }
      if (!this.validateEmail(this.email)) {
        this.error.email = true;
      }
      if (!this.validatePhone(this.phone)) {
        this.error.phone = true;
      }
      if (!this.error.email && !this.error.name && !this.error.phone) {
        let user = {
          name: this.name,
          email: this.email,
          phone: this.phone,
        };
        this.$emit("handleRegister", user);
      }
    },
    handleFocus(value) {
      this.error[value] = false;
    },
    validateEmail(email) {
      let mailformat =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      if (email.match(mailformat)) {
        return true;
      }
      return false;
    },
    validatePhone(phone) {
      let phoneformat = /^\d+$/;
      if (phone.match(phoneformat) && phone.length > 8) {
        return true;
      }
      return false;
    },
  },
};
</script>

<style lang="scss" scoped>
.register {
  &-form {
    position: relative;
    top: 100px;
    width: 100%;
    max-width: 600px;

    height: auto;
    background: white;
    margin: 0 auto;
    padding: 25px;
    border-radius: 10px;
  }
  &-submit {
    display: flex;
    justify-content: center;

    button {
      outline: none;
      border: 0;
      color: white;
      background: #bb2327;
      font-size: 18px;
      font-weight: 400;
      padding: 10px 25px;
      cursor: pointer;
      border-radius: 10px;
    }
  }
}
input {
  -webkit-user-select: text; /* Chrome, Opera, Safari */
  -moz-user-select: text; /* Firefox 2+ */
  -ms-user-select: text; /* IE 10+ */
  user-select: text; /* Standard syntax */
  font-family: "Poppins", sans-serif;
  font-size: 18px;
  padding: 10px 15px;

  width: 100%;
  background: #ecf0f1;
  border: 0;
  outline: 2px solid transparent;
  border-radius: 10px;
  &:focus {
    outline-color: #2c3e50;
  }
  &::placeholder {
    font-weight: 400;
  }
}
.input-filed {
  margin-bottom: 25px;
}
.show-message {
  font-weight: 300;
  padding-top: 5px;
  color: #bb2327;
}

.validateFiled {
  outline-color: #bb2327;
}
.banner {
  width: 100%;
  display: flex;
  justify-content: right;
  img {
    width: 150px;
  }
  padding-bottom: 25px;
}
</style>
