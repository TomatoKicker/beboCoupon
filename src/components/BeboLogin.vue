<template>
  <div class="container">
    <div class="mb-3">
      <label for="exampleInputPassword1" class="form-label">Password</label>
      <input
        type="password"
        class="form-control"
        id="exampleInputPassword1"
        v-model="password" />
    </div>
    <button type="button" @click="login" class="btn btn-primary">login</button>
  </div>
</template>

<script>
import { ref } from "vue";
import supabase from "./supabase";

export default {
  name: "BeboLogin",
  props: {},
  components: {},
  data() {
    return {};
  },
  async mounted() {},
  setup(props, { emit }) {
    let password = ref();

    async function login() {
      const { data, error } = await supabase.auth.signInWithPassword({
        email: process.env.VUA_APP_SUPABASE_LOGIN_MAIL,
        password: password.value,
      });

      if (error) {
        console.log("login failed");
        console.log(error);
      }

      if (data) {
        console.log("login successfull");
        console.log(data);
        emit("loggedInEvent");
      }
    }

    return {
      password,
      login,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  height: 100%;
}
</style>
