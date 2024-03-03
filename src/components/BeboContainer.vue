<template>
  <BeboMain v-if="loggedIn" />
  <BeboLogin v-if="!loggedIn" @loggedInEvent="refreshStatus" />
</template>

<script>
import { ref } from "vue";
import supabase from "./supabase";
import { useToast } from "vue-toast-notification";
import "vue-toast-notification/dist/theme-sugar.css";
import BeboLogin from "./BeboLogin.vue";
import BeboMain from "./BeboMain.vue";

export default {
  name: "BeboContainer",
  props: {
    msg: String,
  },
  components: {
    BeboLogin,
    BeboMain,
  },
  data() {
    return {
      coupons: null,
    };
  },
  async mounted() {
    const user = await supabase.auth.getSession();
    if (user.data?.session) {
      const $toast = useToast();
      $toast.success("You did it!");
      this.refreshStatus();
    }
  },
  setup() {
    const loggedIn = ref();
    function refreshStatus() {
      loggedIn.value = true;
    }

    return {
      loggedIn,
      refreshStatus,
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
