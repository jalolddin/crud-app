<template>
  <div class="updateUser">
    <h1 class="updateUser__title">Обновить пользователя</h1>

    <!--  ============  Form ========= -->

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

    <!--    ===============      Form end ========== -->
    
    <!-- ========   Create Buttons === -->
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
      name: '',
      email: '',
      requiredName: false,
      requiredEmail: false,
    };
  },
  //  ========   Auth Key
  computed: {
    auth_key(){
return  JSON.parse(localStorage.getItem('user_key'))
    },
  },

  //  ========  Input Values
  
  created(){
    this.name = JSON.parse(localStorage.getItem('name')) ?? this.$store.state.name
    this.email = JSON.parse(localStorage.getItem('email')) ?? this.$store.state.email
  },

  //  ===========  Senda Data to url
  
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
                Authorization: "Bearer " + this.auth_key,
            },
          }
        )
        .then((response) => {
          if (response.status == 200) {
            this.$toast.success(`Данные успешно сохранены`, {
              position: "top-right",
            });
            localStorage.setItem('name', JSON.stringify(response.data.name))
            localStorage.setItem('email', JSON.stringify(response.data.email))
            this.$router.push("/");
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
