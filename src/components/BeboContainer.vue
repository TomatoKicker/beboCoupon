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
  <BeboLogin />
</template>

<script>
import { reactive } from "vue";
import supabase from "./supabase";
import BeboImageButton from "./BeboImageButton.vue";
import BeboLogin from "./BeboLogin.vue";

export default {
  name: "BeboContainer",
  props: {
    msg: String,
  },
  components: {
    BeboImageButton,
    BeboLogin,
  },
  data() {
    return {
      coupons: null,
    };
  },
  async mounted() {
    this.coupons = await this.fetchDBData();
    this.reactiveAmount.kisses = this.coupons[0].amount;
    this.reactiveAmount.hugs = this.coupons[1].amount;
    this.reactiveAmount.tableCleaning = this.coupons[2].amount;
    this.reactiveAmount.massage = this.coupons[3].amount;
    this.reactiveAmount.tickle = this.coupons[4].amount;
  },
  setup() {
    const reactiveAmount = reactive({
      hugs: 0,
      kisses: 0,
      tableCleaning: 0,
      massage: 0,
      tickle: 0,
    });
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
    return {
      fetchDBData,
      reactiveAmount,
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
