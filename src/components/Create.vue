<template>
  <div>
    <h1>Add New Item</h1>
    <v-container align-center="true">
      <v-layout column>
        <input v-model="productName" placeholder="Product Name">
        <input v-model="productImage" placeholder="Product Image URL">
        <v-btn color="info" v-on:click="addProduct">Create Product</v-btn>
        <v-img v-if="productImage && active" class="rotate" :src="productImage"></v-img>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import Vue from "vue";

import axios from "axios";

export default {
  name: "Create",
  components: {},
  data() {
    return {
      productName: null,
      productImage: null,
      active: false,
      product: undefined,
      format: { height: "400", width: "5" },
      token:
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7Il9pZCI6IjVjMWQ0YzAyYzExODEzMDAxNjUwYjBhZSIsIm5hbWUiOiJWaWN0b3JpYSBUZXN0Q28iLCJlbWFpbCI6InZpY3RvcmlhbGJyZXdlckBnbWFpbC5jb20iLCJwaG9uZSI6IjU0MTk2ODYxNTEiLCJhdmF0YXIiOiIiLCJjb21wYW55IjoiNWMxZDRjMDExNGFjNGIwMDE2ODE4MDdmIiwicGhvbmVfdmVyaWZpZWQiOmZhbHNlLCJhY3RpdmUiOnRydWUsImxvY2FsZSI6ImVuX1VTIiwiZ3JvdXBzIjpbeyJsYWJlbF9jb2xvciI6IiM3MzdmZmEiLCJ1c2VycyI6W10sIl9pZCI6IjVjMWQ0YzAxMTRhYzRiMDAxNjgxODA4MCIsIm5hbWUiOiJBZG1pbiIsInBlcm1pc3Npb25zIjp7ImFkbWluIjp0cnVlfSwiY29tcGFueSI6IjVjMWQ0YzAxMTRhYzRiMDAxNjgxODA3ZiIsImNyZWF0ZWRBdCI6IjIwMTgtMTItMjFUMjA6MjQ6MzMuODkyWiIsInVwZGF0ZWRBdCI6IjIwMTgtMTItMjFUMjA6MjQ6MzMuODkyWiIsIl9fdiI6MH1dLCJsYW5ndWFnZV9vdmVycmlkZXMiOnt9fSwiaWF0IjoxNTU5OTM3OTc0fQ.DmWjursWGG4qKuW5sBUSC2XcMXk_vvUO2KGT4xMTNEc" // brand: "5bf4456457cf55001f0ebb0e",
    };
  },
  beforeMount() {},
  methods: {
    async addProduct() {
      this.active = true;
      const criteria = {
        bbtoken: this.token,
        name: this.productName,
        techpack_images: [
          {
            image: {
              front_page: true,
              url: this.productImage,
              name: this.productName
            }
          }
        ],
        files: []
      };
      const resp = await axios.post(
        "https://parkcurity.herokuapp.com/product",
        criteria
      );
      this.productName = "";
      this.product = resp.data.data;
      console.log(this.product);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
input {
  border: 1px #e8e8e8 solid;
  padding: 10px;
  margin: 10px;
}
.info {
  background-color: #2196f3 !important;
  border-color: #2196f3 !important;
  margin-bottom: 100px;
}
.rotate {
  animation: rotation 2s infinite linear;
  width: 300px;
  text-align: center;
  margin-top: 100px;
  margin: auto;
}
@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}
</style>
