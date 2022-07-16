<template>
  <div class="col-md-12">
    <div class="card card-container">
      <!-- <img
        id="profile-img"
        src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
        class="profile-img-card"
      /> -->
      <Form @submit="handleRegister" :validation-schema="schema">
        <div v-if="!successful">
          <div class="form-group">
            <label for="username">Логин</label>
            <Field name="username" type="text" class="form-control" />
            <ErrorMessage name="username" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="email">E-Mail</label>
            <Field name="email" type="email" class="form-control" />
            <ErrorMessage name="email" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="password">Пароль</label>
            <Field name="password" type="password" ref="password" class="form-control" />
            <ErrorMessage name="password" class="error-feedback" />
          </div>
          <div class="form-group">
            <label for="password_repeat">Повторите пароль</label>
            <Field name="password_repeat" type="password" class="form-control" />
            <ErrorMessage name="password_repeat" class="error-feedback" />
          </div>

          <div class="form-group">
            <button class="btn btn-primary btn-block button button_login" :disabled="loading">
              <span
                v-show="loading"
                class="spinner-border spinner-border-sm"
              ></span>
              Продолжить
            </button>
          </div>
        </div>
      </Form>

      <div
        v-if="message"
        class="alert"
        :class="successful ? 'alert-success' : 'alert-danger'"
      >
        {{ message }}
      </div>
    </div>
  </div>
</template>

<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import * as yup from "yup";

export default {
  name: "Register",
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  data() {
    const schema = yup.object().shape({
      username: yup
        .string()
        .required("Требуется логин!")
        .min(3, "Минимум 3 символа!")
        .max(20, "Максимум 20 символов!"),
      email: yup
        .string()
        .required("Требуется E-Mail!")
        .email("E-Mail недействителен!")
        .max(50, "Максимум 50 символов!"),
      password: yup
        .string()
        .required("Требуется пароль!")
        .min(6, "Минимум 6 символов!")
        .max(40, "Максимум 40 символов!"),
      password_repeat: yup
        .string()
        .required("Подтвердите пароль!")
        .oneOf([yup.ref("password")], "Пароли не совпадают!"),
    });

    return {
      successful: false,
      loading: false,
      message: "",
      schema,
    };
  },
  computed: {
    loggedIn() {
      return this.$store.state.auth.status.loggedIn;
    },
  },
  mounted() {
    if (this.loggedIn) {
      this.$router.push("/profile");
    }
  },
  methods: {
    handleRegister(user) {
      this.message = "";
      this.successful = false;
      this.loading = true;

      this.$store.dispatch("auth/register", user).then(
        (data) => {
          this.message = data.message;
          this.successful = true;
          this.loading = false;
        },
        (error) => {
          this.message =
            (error.response &&
              error.response.data &&
              error.response.data.message) ||
            error.message ||
            error.toString();
          this.successful = false;
          this.loading = false;
        }
      );
    },
  },
};
</script>

<style scoped>

label {
  color: #7809e8;
  display: block;
  font-size: 16px;
  font-weight: bold;
  text-transform: uppercase;
}

.card-container.card {
  max-width: 350px !important;
  padding: 40px 40px;
}

.card {
  background-color: white;
  padding: 20px 25px 30px;
  margin: 0 auto 25px;
  margin-top: 50px;
  -moz-border-radius: 2px;
  -webkit-border-radius: 2px;
  border-radius: 2px;
  -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
}

.profile-img-card {
  width: 96px;
  height: 96px;
  margin: 0 auto 10px;
  display: block;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
}

.error-feedback {
  color: red;
}

.container {
    background-color: #7809e8;
    color: white;
    font: Sans-serif;
    align-items: center;
}

.inner_container {
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
    background-color: white;
}

.text {
    color: #7809e8;
}

.field {
    background-color: #f2f3f6;
    border: none;
    color: #b8b9bf;
}

.button {
    border: none;
    border-radius: 20px;
    height: 40px;
    margin-top: 20px;
}
.button:hover {
  background-color: #CF9FFF;
}
.button_login {
    background-color: #7809e8;
    color: white;
}
.button_register {
    margin-top: 5px;
    background-color: #f2f3f6;
    color: #7809e8;
}
.button_noacc {
    margin-top: none;
    background: none;
    color: #b8b9bf;
}

</style>
