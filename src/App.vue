<template>
  <div class="background" id="app">
    <header>
      <button id="checkoutbutton" v-if="cartCheck" v-on:click="showCheckout">
        {{ cartItemCount }} <span class="fas fa-cart-plus"></span> Checkout
      </button>
      <button id="checkoutdisabled" v-else disabled>
        {{ cartItemCount }} <span class="fas fa-cart-plus"></span> Checkout
      </button>
    </header>

    <main>
      <div class="filter" v-if="showProduct">
        <h1 style="text-align: center">After School Lessons</h1>

        <!-- Displaying lessons & data in HTML -->
        <Lesson
          v-for="product in products"
          :key="product.subject"
          :product="product"
          v-on:can-add-to-cart="canAddToCart"
          v-on:add-to-cart="addToCart"
          v-on:delete-from-cart="deletefromcart"
          v-on:cart-count="cartCount"
        />
      </div>

      <!-- Displaying information in the checkout page -->
      <div v-else>
        <Checkout
          :cart="cart"
          v-on:cart-check="cartCheck"
          v-on:cart-count="cartCount"
          v-on:delete-from-cart="deletefromcart"
          v-on:submit-form="submitForm"
        />
      </div>
    </main>
  </div>
</template>

<script>
import Lesson from "./components/Lesson.vue";
import Checkout from "./components/Checkout.vue";

export default {
  name: "App",
  components: {
    Lesson,
    Checkout,
  },
  data() {
    return {
      products: [
        {
          id: 1001,
          subject: "Art",
          location: "Mirdiff",
          price: 1000,
          availableInventory: 5,
          image: "art.png",
        },
        {
          id: 1002,
          subject: "Singing",
          location: "Deira",
          price: 1200,
          availableInventory: 5,
          image: "singing.jpg",
        },
        {
          id: 1003,
          subject: "Gymnastics",
          location: "Jabel Ali",
          price: 2000,
          availableInventory: 5,
          image: "gym.jpg",
        },
        {
          id: 1004,
          subject: "Dancing",
          location: "Palm Jumeirah",
          price: 800,
          availableInventory: 5,
          image: "dancing.jpg",
        },
        {
          id: 1005,
          subject: "Chess",
          location: "Al Safouh",
          price: 500,
          availableInventory: 5,
          image: "chess.png",
        },
        {
          id: 1006,
          subject: "Robotics",
          location: "Downtown Dubai",
          price: 2500,
          availableInventory: 5,
          image: "robotics.jpg",
        },
        {
          id: 1007,
          subject: "Debate",
          location: "Al Barsha",
          price: 600,
          availableInventory: 5,
          image: "debate.jpg",
        },
        {
          id: 1008,
          subject: "Yoga",
          location: "Al Corniche",
          price: 1700,
          availableInventory: 5,
          image: "yoga.jpg",
        },
        {
          id: 1009,
          subject: "Swimming",
          location: "Al Garhoud",
          price: 900,
          availableInventory: 5,
          image: "swimming.jpg",
        },
        {
          id: 1010,
          subject: "Martial Arts",
          location: "Al Mamzar",
          price: 1300,
          availableInventory: 5,
          image: "martialarts.jpg",
        },
      ],
      cart: [],
      showProduct: true,
      carttotal: 0,
      order: {
        firstName: "",
        phoneNumber: "",
      },
    };
  },

  methods: {
    submitForm() {
      alert("Order Submitted!");
    },
    addToCart(product) {
      let baseProduct = {
        id: product.id,
        title: product.subject,
        price: product.price,
        image: product.image,
        quantity: 1,
      };

      let filter = this.cart.filter((lesson) => lesson.id === product.id);

      if (filter.length > 0) {
        if (filter[0].quantity > 0) {
          let index = this.cart.findIndex(
            (object) => object.id === filter[0].id
          );

          this.cart[index].quantity += 1;

          let productIndex = this.products.findIndex(
            (prod) => prod.id === product.id
          );
          this.products[productIndex].availableInventory -= 1;
          this.carttotal += 1;
        }
      } else {
        this.cart.push(baseProduct);

        let productIndex = this.products.findIndex(
          (prod) => prod.id === product.id
        );
        this.products[productIndex].availableInventory -= 1;
        this.carttotal += 1;
      }
    },
    deletefromcart(product) {
      let filter = this.cart.filter((lesson) => lesson.id === product.id);
      let productIndex = this.products.findIndex(
        (prod) => prod.id === product.id
      );

      if (filter[0].quantity > 1) {
        let index = this.cart.findIndex((object) => object.id === filter[0].id);

        this.cart[index].quantity -= 1;
        this.products[productIndex].availableInventory += 1;
        this.carttotal -= 1;
      } else {
        let index = this.cart.findIndex((object) => object.id === filter[0].id);

        if (index > -1) {
          this.cart.splice(index, 1);
          this.products[productIndex].availableInventory += 1;
          this.carttotal -= 1;
        }
      }
    },
    showCheckout() {
      this.showProduct = this.showProduct ? false : true;
    },

    canAddToCart(product) {
      return product.availableInventory > this.cartCount(product.id);
    },
    cartCount(id) {
      let count = 0;

      for (let i = 0; i < this.cart.length; i++) {
        if (this.cart[i] === id) {
          count++;
        }
      }

      return count;
    },
  },
  computed: {
    cartItemCount() {
      return this.carttotal;
    },
    cartCheck() {
      if (this.carttotal > 0) return true;

      return false;
    },
  },
};
</script>

<style>
#checkoutbutton {
  border: none;
  outline: 0;
  padding: 12px;
  color: white;
  background-color: #000;
  text-align: center;
  cursor: pointer;
  width: 300px;
  font-size: 18px;
  margin: 20px;
}
#checkoutdisabled {
  border: none;
  outline: 0;
  padding: 12px;
  color: white;
  background-color: gray;
  text-align: center;
  cursor: pointer;
  width: 300px;
  font-size: 18px;
  margin: 20px;
}
</style>
