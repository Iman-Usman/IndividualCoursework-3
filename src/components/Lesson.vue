<template>
  <div id="card">
    <h2 v-text="product.subject"></h2>
    <figure>
      <img style="width: 100%; height: 200px" v-bind:src="product.image" />
    </figure>
    <p><strong>Location: </strong>{{ product.location }}</p>
    <p><strong>Price: </strong>${{ product.price }}</p>
    <p><strong>Available spaces: </strong>{{ product.availableInventory }}</p>

    <button
      id="button"
      v-on:click="addToCart(product)"
      v-if="canAddToCart(product)"
    >
      Add to cart
    </button>
    <button id="button" disabled="disabled" v-else>Add to cart</button>
  </div>
</template>

<script>
export default {
  name: "Lesson-page",
  props: ["product"],
  methods: {
    addToCart(product) {
      this.$emit("add-to-cart", product);
    },
    canAddToCart(product) {
      return this.$emit("can-add-to-cart", product);
    },
    deletefromcart(product) {
      this.$emit("delete-from-cart", product);
    },
    cartCount(productId) {
      this.$emit("cart-count", productId);
    },
  },
};
</script>
<style>
#card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  width: 20%;
  height: 50%;
  margin: 20px;
  text-align: center;
  font-family: arial;
  display: inline-block;
}
#card button {
  border: none;
  outline: 0;
  padding: 12px;
  color: white;
  background-color: #000;
  text-align: center;
  cursor: pointer;
  width: 100%;
  font-size: 18px;
}
</style>
