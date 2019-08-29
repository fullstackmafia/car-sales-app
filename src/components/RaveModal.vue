<template>
  <div class="rave">
    <button class="button" @click="payWithRave">Book Now</button>
  </div>
</template>

<script>
export default {
  name: "rave",
  props: {
    isProduction: {
      type: Boolean,
      required: false,
      default: false //set to true if you are going live
    },
    email: {
      type: String,
      required: true
    },
    amount: {
      type: Number,
      required: true
    },
    raveKey: {
      type: String,
      required: true
    },
    callback: {
      type: Function,
      required: true,
      default: response => {}
    },
    close: {
      type: Function,
      required: true,
      default: () => {}
    },
    currency: {
      type: String,
      default: "NGN"
    },
    country: {
      type: String,
      default: "NG"
    },
    custom_title: {
      type: String,
      default: ""
    },
    custom_logo: {
      type: String,
      default: ""
    },
     reference: {
          type: String,
          required: true
      },
    payment_method: {
      type: String,
      default: ""
    }
  },
  created() {
    const script = document.createElement("script");
    script.src = !this.isProduction
      ? "https://ravesandboxapi.flutterwave.com/flwv3-pug/getpaidx/api/flwpbf-inline.js"
      : "https://api.ravepay.co/flwv3-pug/getpaidx/api/flwpbf-inline.js";
    document.getElementsByTagName("head")[0].appendChild(script);
  },
  methods: {
    payWithRave() {
      window.getpaidSetup({
        customer_email: this.email,
        amount: this.amount,
        txref: this.reference,
        PBFPubKey: this.raveKey,
        onclose: () => this.close(),
        callback: response => this.callback(response),
        currency: this.currency,
        country: this.country,
        custom_title: this.custom_title,
        custom_logo: this.custom_logo,
        payment_method: this.payment_method,
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.button {
  background-color: hsl(200, 100%, 70%);
  color: hsl(0, 0%, 15%);
  width: 100px;
  height: 40px;
  font-weight: 800;
  border-radius: 5px;
  opacity: 0.7;
  transition: 0.5s;
}
.button:hover {
  opacity: 1;
}
</style>
