<script>
import axios from "axios";
// import { assertExpressionStatement } from "@babel/types";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      name: "Erwin",
      products: [],
      paramsProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products.json").then((response) => {
        this.products = response.data;
        console.log(this.products);
      });
    },
    createProduct: function () {
      axios
        .post("http://localhost:3000/products.json", this.paramsProduct)
        .then((response) => {
          console.log("Product Added", response.data);
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>{{ name }}</h2>

    <div>
      <button v-on:click="createProduct">Create Product</button>
    </div>
    <div>
      name:
      <input type="text" v-model="paramsProduct.name" />
      description:
      <input type="text" v-model="paramsProduct.description" />
      image_url:
      <input type="text" v-model="paramsProduct.image_url" />
      price:
      <input type="text" v-model="paramsProduct.price" />
    </div>

    <div v-for="product in products" v-bind:key="product.id">
      <p>Name: {{ product.name }}</p>
      <p>Price: {{ product.price }}</p>
      <p>Description: {{ product.description }}</p>
      <!-- <img v-bind:src="product.image_url" /> -->
      <!-- same as above but does require v-bind to be typed in with alt for name if image doesn't load -->
      <img :src="product.image_url" :alt="product.name" />
    </div>
  </div>
</template>

<style>
img {
  width: 300px;
  border-radius: 8px;
}
</style>
