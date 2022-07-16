<template>
  <div class="col-md-12">
    <div class="card card-container">
      <!-- <img
        id="profile-img"
        src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
        class="profile-img-card"
      /> -->
      <Form @submit.prevent>
        <div v-if="!successful">

          <div class="form-group">
            <label class="bbc" for="choose_your_role">организатор</label>
            <div class="role_buttons_box">
                <button :class="[kuratorRolePicked ? 'is_clicked' : 'isnt_clicked']" 
                @click="changeMessage('информация о кураторе'); changeRoleToKurator();" 
                class="kuratorRolePicked btn btn-primary button button_role">
                куратор
                </button>
                <button :class="[sozdatelRolePicked ? 'is_clicked' : 'isnt_clicked']" 
                @click="changeMessage('информация о создателе'); changeRoleToSozdatel();" 
                class="sozdatelRolePicked btn btn-primary button button_role">
                создатель
                </button>
            </div>
            <label v-bind:value="role_reveal_text" class="b role_reveal_text">{{ role_reveal_text }}</label>
          </div>

          <div class="form-group">
            <label class="bbc" for="choose_your_role">организатор-куратор</label>
            <div class="role_buttons_box">
                <button :class="[physicalRolePicked ? 'is_clicked' : 'isnt_clicked']" 
                @click="changeMessage2('информация о физ. лице'); changeRoleToPhysical();" 
                class="physicalRolePicked btn btn-primary button button_role">
                физ. лицо
                </button>
                <button :class="[legalRolePicked ? 'is_clicked' : 'isnt_clicked']" 
                @click="changeMessage2('информация о юр. лице'); changeRoleToLegal();" 
                class="legalRolePicked btn btn-primary button button_role">
                юр. лицо
                </button>
            </div>
            <label v-bind:value="role_reveal_text2" class="b role_reveal_text2">{{ role_reveal_text2 }}</label>
          </div>

          <div class="form-group" style="margin-top: 24px;">
            <button @click="handleOrganizatorSettings" class="bbc btn btn-primary btn-block button button_register" :disabled="loading">
              <span
                v-show="loading"
                class="spinner-border spinner-border-sm"
              ></span>
              Зарегистрироваться
            </button>
            <button class="bbc btn btn-primary btn-block button button_back">
              Вернуться
            </button>
          </div>

          <div class="form-group">
            <label class="note_text">Нажимая на кнопку регистрации Вы даёте <router-link to="/agreement" active-class="active" class="agreement_link_text">согласие на обработку персональных данных</router-link></label>
          </div>

        </div>
      </Form>

    </div>
  </div>
</template>

<script>

export default {
  name: "OrganizatorSettings",
  data() {
    return {
        kuratorRolePicked: true,
        sozdatelRolePicked: false,
        physicalRolePicked: true,
        legalRolePicked: false,
        role_reveal_text: 'информация о кураторе',
        role_reveal_text2: 'информация о физ. лице',
    }
  },
  
  methods: {
    changeRoleToKurator() {
        if (!this.kuratorRolePicked) {
        this.kuratorRolePicked = !this.kuratorRolePicked;
        this.sozdatelRolePicked =!this.sozdatelRolePicked;
        }
    },
    changeRoleToSozdatel() {
        if (!this.sozdatelRolePicked) {
        this.kuratorRolePicked = !this.kuratorRolePicked;
        this.sozdatelRolePicked =!this.sozdatelRolePicked;
        }
    },
    changeRoleToPhysical() {
        if (!this.physicalRolePicked) {
        this.physicalRolePicked = !this.physicalRolePicked;
        this.legalRolePicked =!this.legalRolePicked;
        }
    },
    changeRoleToLegal() {
        if (!this.legalRolePicked) {
        this.physicalRolePicked = !this.physicalRolePicked;
        this.legalRolePicked =!this.legalRolePicked;
        }
    },
    changeMessage(message) {
      this.role_reveal_text = message;
    },
    changeMessage2(message) {
      this.role_reveal_text2 = message;
    },
    handleOrganizatorSettings(user) {
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

.b {
    font-weight: bold;
}

label {
  color: #7809e8;
  text-align: center;
  display: block;
  margin-top: 10px;
}
.role_reveal_text, .role_reveal_text2 {
    color: #a5a6ad;
}
.note_text {
    color: #a5a6ad;
    font-size: 12px;
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
.role_buttons_box {
    background-color: aqua;
    display: grid;
    grid-template-columns: 1fr 1fr;
    margin-top: 0px;
    border-radius: 10px;
    background-color: #f2f3f6;
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
.button_back {
    background-color: #f2f3f6;
    color: #7809e8;
}
.button_register {
    background-color: #7809e8;
    color: white;
    margin-top: 5px;
}
.button_role {
    margin-top: 0px;
    border-radius: 10px;
}
.is_clicked {
    background-color: #7809e8;
    color: white;
}
.isnt_clicked {
    background: none;
    color: #bdbec4;
}
.button_role_player {
    border-top-right-radius: 0px;
    border-bottom-right-radius: 0px;
}
.button_role_organizer {
    border-top-left-radius: 0px;
    border-bottom-left-radius: 0px;
}

</style>
