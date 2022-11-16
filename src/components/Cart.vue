<template>
  <div class="row sticky-top">
    <div class="col-md-12">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">Commande</h5>
          <div class="card-text">
            <ul class="list-group">
              <li class="list-group-item" v-for="item in cart">
                {{ item.name }} - {{ item.quantity }} x {{ item.price }}$
                <button
                  class="btn btn-close btn-sm"
                  @click="removeFromCart(item)"
                />
              </li>
            </ul>
          </div>
        </div>

        <div class="row" v-if="cart.length > 0">
          <div class="d-grid col mx-3">
            <button class="btn btn-success" @click="isOrdered = !isOrdered">
              Commander
            </button>
          </div>
          <div class="d-grid col gap-2 py-2">Total: {{ totalPrice }}$</div>
        </div>

        <div class="row" v-if="cart.length == 0">
          <div class="col-md-12">
            <div class="alert alert-info">Votre panier est vide</div>
          </div>
        </div>

        <div class="row" v-if="isOrdered">
          <div class="col-md-12">
            <div class="card-body">
              <form @submit.prevent="order">
                <div class="form-group">
                  <input
                    type="text"
                    v-model="email"
                    class="form-control"
                    id="email"
                    name="email"
                    placeholder="Courriel"
                  />
                </div>
                <div class="form-group mt-2">
                  <button type="submit" class="btn btn-primary">
                    Soumettre
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["cart"],
  data() {
    return {
      isOrdered: false,
      email: "",
    };
  },
  watch: {},
  computed: {
    totalPrice() {
      this.isOrdered = false;
      return this.cart.reduce((total, item) => {
        return total + item.price * item.quantity;
      }, 0);
    },
  },
  updated() {
    console.log(this.cart);
  },
  methods: {
    order() {
      if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.email) || this.email == "") {
        alert("Veuillez entrer un courriel valide");
        return;
      }
      fetch(
        "https://d10-ex-default-rtdb.europe-west1.firebasedatabase.app/orders.json",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify([...this.cart, { email: this.email }]),
        }
      )
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          alert("Commande effectuée");
          this.cart.splice(0, this.cart.length);
          this.isOrdered = false;
          this.email = "";
        })
        .catch((error) => alert("il y a eu une erreur lors de votre commande"));
    },
    removeFromCart(item) {
      this.cart.splice(this.cart.indexOf(item), 1);
    },
  },
};
</script>

<style scoped></style>
