<template>
  <div
    class="container d-flex justify-content-center"
    style="min-height: 100vh">
    <div class="row my-auto">
      <div class="col-12">
        <div class="mb-3">
          <input
            type="password"
            class="form-control"
            id="exampleInputPassword1"
            v-model="password" />
        </div>
        <button type="button" @click="login" class="btn btn-primary fw-bold">
          LOGIN
        </button>
      </div>
    </div>
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
        email: process.env.VUE_APP_SUPABASE_LOGIN_MAIL,
        password: password.value,
      });

      if (error) {
        console.log("login failed");
        console.log(error);
      }

      if (data.session) {
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
.btn-primary {
  border: 1px solid #ffb9d2;
  background-color: #ffb9d2;
}
</style>
