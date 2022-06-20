<template>
  <div>
    <form action="" method="POST" @submit.prevent="login">
      <div>
        <input
          type="email"
          v-model="username"
          name="username"
          id="username"
          placeholder="Email"
        />
      </div>

      <div>
        <input
          type="password"
          v-model="password"
          name="password"
          id="password"
          placeholder="Enter your password"
        />
      </div>

      <div>
        <button type="submit">Login</button>
      </div>
    </form>
  </div>
</template>

<script>
import gql from "graphql-tag";
import { onLogin } from "../vue-apollo.js";

export default {
  data() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    login() {
      this.$apollo
        .mutate({
          mutation: gql`
            mutation ($data: LoginInput!) {
              login(data: $data) {
                access_token
              }
            }
          `,
          variables: {
            data: {
              username: this.username,
              password: this.password,
            },
          },
        })
        .then((data) => {
          console.log(data);
          onLogin(
            this.$apollo.provider.defaultClient,
            data.data.login.access_token
          );
          this.$router.push("/homeview");
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
};
</script>

<style scoped>
input {
  padding: 14px 20px;
  font-size: 18px;
  margin-bottom: 14px;
}

button {
  width: 160px;
  padding: 14px;
  background: #017bff;
  color: white;
  font-size: 16px;
}
</style>
