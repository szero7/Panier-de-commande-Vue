<template>
  <div class="col-md-6 col-sm-12">
    <div class="border bg-light mb-4">
      <div class="card-body">
        <!-- picture -->
        <div class="row">
          <div class="col-md-12">
            <img :src="product.picture" class="img-fluid" alt="" />
          </div>
        </div>
        <h5 class="card-title">{{ product.name }}</h5>
        <p class="card-text">{{ product.price }}$</p>
        <!-- row 2 cols -->
        <div class="row">
          <div class="col-md-8">
            <label :class="{invalid:invalidQuantity}">Quantity:
              <input type="number" v-model="quantity" min="0" max="10" />
            </label>
          </div>
          <div class="col-md-4">
            <button
              class="btn"
              :class="quantity > 0 ? 'btn-success' : 'btn-primary'"
              @click="addToCart()"
              :disabled="quantity < 1 || quantity > 10">
              Add product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["product"],
  watch: {
    quantity(value) {
      if (value < 0 || value > 10) {
        this.invalidQuantity = true;
      } else {
        this.invalidQuantity = false;
      }
    },
  },
  data() {
    return {
      quantity: 0,
      invalidQuantity: false,
    };
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", {
        id: this.product.id,
        name: this.product.name,
        price: this.product.price,
        quantity: this.quantity,
      });
      this.quantity = 0;
    },
  },
};
</script>

<style scoped>
input {
  width: 70px;
}
 .invalid {
  color: red;
}
</style>
