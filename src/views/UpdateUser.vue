<template>
  <div class="updateUser">
    <h1 class="updateUser__title">Обновить пользователя</h1>

    <!-- Form -->

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
    <div class="createUser__buttons">
      <button @click="sendData">Обновить</button>
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
      name: this.$store.state.name,
      email: this.$store.state.email,
      requiredName: false,
      requiredEmail: false,
    };
  },
  methods: {
    async sendData() {
      if (this.name == "") {
        this.requiredName = true;
        this.$refs.name.focus();
        return;
      } else {
        this.requiredName = false;
      }
      if (this.email == "") {
        this.requiredEmail = true;
        this.$refs.focus();
        return;
      } else {
        this.requiredEmail = false;
      }
      await axios
        .patch(
          "https://api.sitemap-generator.ru/test-api/user",
          {
            name: this.name,
            email: this.email,
          },
          {
            headers: {
                Authorization: "Bearer " + this.$store.state.auth,
            },
          }
        )
        .then((response) => {
          if (response.status == 200) {
            this.$toast.success(`Данные успешно сохранены`, {
              position: "top-right",
            });
            this.$router.push("/");
            return;
          }
        })
        .catch((error) => {
          this.$toast.error(`Ошибка с данными`, {
            position: "top-right",
          });
        });

      this.name = "";
      this.email = "";
    },
  },
};
</script>

<style></style>
