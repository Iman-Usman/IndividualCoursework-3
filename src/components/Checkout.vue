<template>
  <div id="checkout">
    <h2>Checkout</h2>
    <div
      id="checkoutcard"
      class="baseProduct"
      v-for="lesson in cart"
      :key="lesson.title"
    >
      <figure>
        <img style="width: 100%; height: 100px" v-bind:src="lesson.image" />
      </figure>
      <p>Subject: {{ lesson.title }}</p>
      <p>Price: ${{ lesson.price }}</p>
      <p>Space: {{ lesson.quantity }}</p>

      <button v-on:click="deletefromcart(lesson)">
        <i class="fa fa-trash"></i>-
      </button>
    </div>

    <p>
      <strong>First Name:</strong>
      <input v-model="order.firstName" />&nbsp;&nbsp;&nbsp;
      <strong>Phone Number:</strong>
      <input v-model="order.phoneNumber" />

      <button id="placeorder" v-on:click="submitForm" v-if="cartCheck">
        Place Order
      </button>
    </p>
  </div>
</template>

<script>
export default {
  name: "Checkout-page",
  props: ["cart", "products", "showProduct"],
  data() {
    return {
      order: {
        firstName: "",
        phoneNumber: "",
      },
    };
  },

  methods: {
    submitForm() {
      this.$emit("submit-form");
    },
    deletefromcart(product) {
      this.$emit("delete-from-cart", product);
    },
    cartCount(productId) {
      this.$emit("cart-count", productId);
    },
    cartCheck() {
      this.$emit("cart-check");
    },
  },
};
</script>
<style>
#checkoutcard {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  width: 15%;
  height: 30%;
  margin: 5px;
  text-align: center;
  font-family: arial;
  display: inline-block;
}
#placeorder {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  width: 10%;
  background-color: black;
  color: white;
  height: 30%;
  margin: 5px;
  text-align: center;
  font-family: arial;
  display: inline-block;
}
#checkout {
  margin-left: 20px;
}
</style>