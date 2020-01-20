<template>
  <form v-on:submit.prevent="submitForm">
    <!-- 이벤트 기본 새로고침을 막겠다 -->
    <div>
      <label for="username">id : </label>
      <input v-model="username" id="username" type="text" />
    </div>
    <div>
      <label for="password">pw : </label>
      <input v-model="password" id="password" type="password" />
    </div>
    <button type="submit">login</button>
  </form>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      username: "",
      password: ""
    };
  },
  methods: {
    submitForm: function() {
      const url = "http://jsonplaceholder.typicode.com/users";
      const data = {
        username: this.username,
        password: this.password
      };
      axios
        .post(url, data)
        // eslint-disable-next-line no-console
        .then(res => console.log(res))
        .catch();
    }
  }
};
</script>

<style></style>
