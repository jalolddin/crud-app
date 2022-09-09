<template>
  <div class="createUser">
    <h1 class="createUser__title">Создать пользователя</h1>
    <form class="formUser" action="#">
      <!-- Name -->
      <label for="Name">ФИО</label>
      <input
        ref="name"
        v-model="name"
        required
        type="text"
        placeholder="Иванов Иван Иванович"
      />
      <p v-if="requiredName" class="required">Пожалуйста, введите ФИО...</p>
      <!-- Email -->
      <label for="Email">Email</label>
      <input
        ref="email"
        v-model="email"
        required
        type="text"
        placeholder="exmple@gmail.com"
      />
      <p v-if="requiredEmail" class="required">Пожалуйста, введите email...</p>
    </form>

    <!-- Submit Buttons -->

    <div class="createUser__buttons">
      <button @click="sendData">Сохранить</button>
      <router-link to="/">
        <button>Отменить</button>
      </router-link>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {

  data() {
    return {
      name: "",
      email: "",
      requiredName: false,
      requiredEmail: false,
    };
  },
  methods: {
    async sendData() {
      // Validation
      if (this.name == "") {
        this.requiredName = true;
        this.$refs.name.focus();
        return;
      } else {
        this.requiredName = false;
      }
      if (this.email == "") {
        this.requiredEmail = true;
        this.$refs.email.focus();
        return;
      } else {
        this.requiredEmail = false;
      }
      // Request
      await axios
        .post(
          "https://api.sitemap-generator.ru/test-api/user",
          // Send Data
          {
            name: this.name,
            email: this.email,
          },
          {
            headers: {
              "X-Application-Token": "wefiEFv_dwDEvf-12Veda_feadvkJbBgh831",
              "content-type": "application/json",
            },
          }
        )
        .then((response) => {
          if (response.status == 201) {
            this.$toast.success(`Данные успешно сохранены`, {
              position: "top-right",
            });
            localStorage.setItem('value', JSON.stringify(false));
            localStorage.setItem('user_key', JSON.stringify(response.data.auth_key))
            this.name = "";
            this.email = "";
            this.$router.push("/");
          }
     this.$store.state.auth = response.data.auth_key    
     this.$store.state.name = response.data.name
     this.$store.state.email = response.data.email
    })
        .catch((error) => {
          this.$toast.error(`Ошибка с данными`, {
            position: "top-right",
          });
        });
    },
  },
};
</script>

<style></style>
