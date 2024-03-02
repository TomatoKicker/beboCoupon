<template>
  <div class="container">
    <div class="col-12">
      <div class="row">
        <div class="col-12">
          <img src="../assets/bebo.svg" alt="" class="img-fluid pb-5" />
        </div>
      </div>
      <div class="row">
        <div v-for="coupon in coupons" :key="coupon" class="col-6 mx-auto">
          <BeboImageButton :coupon="coupon" />
        </div>
      </div>
    </div>
  </div>
  <div class="btn btn-primary" @click="logout">logout</div>
</template>

<script>
import { ref } from "vue";
import supabase from "./supabase";
import BeboImageButton from "./BeboImageButton.vue";

export default {
  name: "BeboMain",
  props: {
    msg: String,
  },
  components: {
    BeboImageButton,
  },
  data() {
    return {
      coupons: null,
    };
  },
  async mounted() {
    this.coupons = await this.fetchDBData();
    const user = await supabase.auth.getSession();
    if (user.data?.session) {
      this.refreshStatus();
    }
    console.log(user.data?.session);
  },
  setup() {
    const loggedIn = ref();
    function refreshStatus() {
      loggedIn.value = true;
    }
    async function fetchDBData() {
      const { data: coupons, error } = await supabase
        .from("coupons")
        .select("*");

      if (error) {
        console.log("error");
        console.log(error);
        return null;
      }

      if (coupons) {
        console.log("coupon");
        console.log(coupons);
        coupons.sort(function (a, b) {
          return a.id - b.id;
        });
        return coupons;
      }
      return null;
    }
    async function logout() {
      const { error } = await supabase.auth.signOut();
      if (error) {
        console.log(error);
      }
    }
    return {
      fetchDBData,
      loggedIn,
      refreshStatus,
      logout,
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
