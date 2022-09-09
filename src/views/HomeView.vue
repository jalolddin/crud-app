<template>
  <div class="home">
    <!-- Title -->
    <h1 class="home__title">Форма для получения и создания пользователя</h1>
    <!-- Buttons for create user -->
    <div class="home__buttons">
      <!-- CreateUser -->
      <router-link v-if="value" to="/CreateUser">
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
 
        <h5>{{ "ФИО: " + listUsers.name }}</h5>
        <h5>{{ "Email: " + listUsers.email }}</h5>
        <h5>{{ "Authkey: " + listUsers.auth_key }}</h5>
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
  computed: {
    auth_key(){
return JSON.parse(localStorage.getItem('user_key'))
    },
value(){
  return JSON.parse(localStorage.getItem('value')) ?? true 
}
  },
  methods: {
    list() {
      axios
        .get("https://api.sitemap-generator.ru/test-api/user", {
          headers: {
            Authorization: "Bearer " + this.auth_key,
          },
        })
        .then((response) => (this.listUsers = response.data));
    },
  },
};
</script>
