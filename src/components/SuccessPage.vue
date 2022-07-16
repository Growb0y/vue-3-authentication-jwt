<template>
  <div class="col-md-12">
    <div class="card card-container">
      <img
        id="profile-img"
        src="../assets/check_mark.png"
        class="profile-img-card"
      />
      <Form @submit="handleOkay">
        <div v-if="!successful">

          <div class="form-group">
            <label class="bbc" for="success">ваш профиль успешно подтвержден!<br>Самое время заполнить его очень важной информацией!</label>
          </div>

          <div class="form-group">
            <button @click="handleOkay" class="bbc btn btn-primary btn-block button button_okay" :disabled="loading">
              <span
                v-show="loading"
                class="spinner-border spinner-border-sm"
              ></span>
              Я в деле!
            </button>
          </div>

        </div>
      </Form>

    </div>
  </div>
</template>

<script>

export default {
  name: "EmailConfirmation",
  data() {
    return {
        
    }
  },
  
  methods: {
    handleOkay(user) {
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

.bbc {
    font-size: 16px;
    font-weight: bold;
    text-transform: uppercase;
}

label {
  color: #7809e8;
  text-align: center;
  display: block;
  margin-top: 10px;
}
.role_reveal_text {
    color: #bdbec4;
}
.note_text {
    color: #b8b9bf;
}
.agreement_link_text {
    color: #7809e8;
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
.button {
    border: none;
    border-radius: 20px;
    height: 40px;
    margin-top: 20px;
}
.button:hover {
  background-color: #CF9FFF;
}
.button_okay {
    background-color: #7809e8;
    color: white;
    margin-top: 5px;
}

</style>
