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
    <div class="mb-3 form-check">
      <input type="checkbox" class="form-check-input" id="exampleCheck1" />
      <label class="form-check-label" for="exampleCheck1">Check me out</label>
    </div>
    <button type="button" @click="login" class="btn btn-primary">Submit</button>
    <button type="button" @click="seeCurrentUser" class="btn btn-primary">
      CurrentUser
    </button>
    {{ password }}
  </div>
</template>

<script>
import { ref } from "vue";
import supabase from "./supabase";
//import supabase from "./supabase";

export default {
  name: "BeboLogin",
  props: {
    msg: String,
  },
  components: {},
  data() {
    return {
      coupons: null,
    };
  },
  async mounted() {},
  setup() {
    let password = ref("");

    async function login() {
      const { data, error } = await supabase.auth.signInWithPassword({
        email: "test@test.de",
        password: password.value,
      });

      if (error) {
        console.log("error");
        console.log(error);
      }

      if (data) {
        console.log("login successfull");
        console.log(data);
      }
    }
    async function seeCurrentUser() {
      const local = await supabase.auth.getSession();
      console.log(local);
    }

    return {
      password,
      login,
      seeCurrentUser,
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
