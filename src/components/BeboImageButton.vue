<template>
  <img
    v-if="coupon?.name"
    style="height: 182px"
    :src="require('../assets/' + coupon?.name + '.jpeg')"
    alt=""
    class="img-Standard img-fluid rounded rounded-4 position-relative t-100 my-2"
    type="button"
    data-bs-toggle="offcanvas"
    :data-bs-target="'#offcanvas_' + coupon.name"
    :aria-controls="'#offcanvas_' + coupon.name" />
  <div
    class="offcanvas offcanvas-bottom"
    tabindex="-1"
    :id="'offcanvas_' + coupon?.name"
    :aria-labelledby="'offcanvas_' + coupon?.name + '_Label'">
    <div class="offcanvas-header">
      <div
        class="h3 offcanvas-title text-white fw-bold text-uppercase"
        :id="'offcanvas_' + coupon?.name + '_Label'">
        Offcanvas {{ coupon?.name }}
      </div>
      <button
        type="button"
        class="btn-close btn-close-white text-reset"
        data-bs-dismiss="offcanvas"
        aria-label="Close"></button>
    </div>
    <div class="offcanvas-body">
      <div class="row">
        <div class="col-12">
          <div class="text-white h1 fw-bold">Anahl der Coupons:</div>
          <p class="h1">
            {{ reactiveAmount.amount }}
          </p>
        </div>
      </div>
      <div class="row">
        <div class="col-12">
          <div class="text-white h1 fw-bold">Inhalt des Coupons:</div>
          <p class="h1 text-white">
            {{ coupon.text }}
          </p>
        </div>
      </div>
      <div class="row">
        <div class="col-6 mx-auto mt-5">
          <div
            class="btn btn-primary fw-bold"
            @click="decreaseCoupon(coupon.name, reactiveAmount.amount)">
            Einlösen
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, onMounted } from "vue";
import supabase from "./supabase";
export default {
  name: "BeboImageButton",
  props: {
    coupon: Object,
  },
  data() {
    return {
      coupons: null,
    };
  },
  setup(props) {
    const amount = ref(23);
    const reactiveAmount = reactive({
      amount: 0,
    });
    async function clicked() {
      alert("clicked");
    }
    async function decreaseCoupon(name, initialAmount) {
      const newAmount = initialAmount - 1;
      const { error } = await supabase
        .from("coupons")
        .update({ amount: newAmount })
        .eq("name", name);
      if (error) {
        console.log(error);
      }
      this.reactiveAmount.amount = newAmount;
    }
    function getImageSrc(imgName) {
      return "../assets/" + imgName + ".jpeg";
    }
    onMounted(() => {
      reactiveAmount.amount = props.coupon.amount;
    });
    return {
      decreaseCoupon,
      clicked,
      getImageSrc,
      amount,
      reactiveAmount,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

p {
  font-weight: 600;
  color: #ffb9d2;
}
.btn-primary {
  border: 1px solid #ffb9d2;
  background-color: #ffb9d2;
}
.offcanvas {
  min-height: 90%;
  background: linear-gradient(
      0deg,
      rgba(255, 255, 255, 0.6),
      rgba(255, 0, 150, 0.6)
    ),
    url("../assets/offcanvasBackground.jpeg");
  background-position: center;
  background-size: cover;
  border-top: unset !important;
}
.offcanvas-body {
  .h1 {
    color: #ffb9d2 !important;
  }
  p:not(.h1) {
    color: white !important;
  }
}
button {
  color: white !important;
}
</style>
