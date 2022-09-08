<template>
  <div class="home">
    <!-- Title -->
    <h1 class="home__title">Форма для получения и создания пользователя</h1>
    <!-- Buttons for create user -->
    <div class="home__buttons">
      <!-- CreateUser -->
      <router-link to="/CreateUser">
        <button>Создать пользователя</button>
      </router-link>

      <!-- UpdateUser -->
      <router-link to="/UpdateUser">
        <button>Обновить пользователя</button>
      </router-link>

      <!-- ListUser -->
      <button @click="list">Список пользователей</button>
    </div>

    <!-- User list -->
    <div v-if="listUsers">
      <div class="home__users">
        <h2>Пользователь :</h2>
        <h2>{{ "ФИО: " + listUsers.name }}</h2>
        <h2>{{ "Email: " + listUsers.email }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "HomeView",
  data() {
    return {
      listUsers: null,
    };
  },
  methods: {
    list() {
      axios
        .get("https://api.sitemap-generator.ru/test-api/user", {
          headers: {
            Authorization: "Bearer " + this.$store.state.auth,
          },
        })
        .then((response) => (this.listUsers = response.data));
    },
  },
};
</script>
