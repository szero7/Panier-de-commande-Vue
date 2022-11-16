<template>
  <div class="row">
    <div class="col-md-8">
      <div class="row">
        <Product
          v-for="product in products"
          :key="product.id"
          :product="product"
          @add-to-cart="addToCart"
        />
      </div>
    </div>
    <div class="col-md-4 mt-5">
      <Cart :cart="cart" />
    </div>
  </div>
</template>

<script>
import Product from "@/components/Product.vue";
import Cart from "@/components/Cart.vue";

export default {
  components: {
    Product,
    Cart,
  },
  data() {
    return {
      products: [],
      cart: [],
    };
  },
  created() {
    this.getPizzas();
  },
  methods: {
    getPizzas() {
      fetch(
        "https://d10-ex-default-rtdb.europe-west1.firebasedatabase.app/pizzas.json"
      )
        .then((response) => response.json())
        .then((data) => {
          this.products = data;
        });
    },
    addToCart(product) {
      this.cart.push(product);
    },
  },
};
</script>

<style scoped></style>
