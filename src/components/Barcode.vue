<template>
  <div>
    <barcode
      :width="this.format.width"
      :height="this.format.height"
      v-bind:value="this.product.code"
    >Show this if the rendering fails.</barcode>
  </div>
</template>

<script>
import Vue from "vue";

import axios from "axios";
import VueBarcode from "vue-barcode";

export default {
  name: "Barcode",
  components: {
    barcode: VueBarcode
  },
  data() {
    return {
      product: undefined,
      format: { height: "400", width: "5" },
      token:
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7Il9pZCI6IjVjMWQ0YzAyYzExODEzMDAxNjUwYjBhZSIsIm5hbWUiOiJWaWN0b3JpYSBUZXN0Q28iLCJlbWFpbCI6InZpY3RvcmlhbGJyZXdlckBnbWFpbC5jb20iLCJwaG9uZSI6IjU0MTk2ODYxNTEiLCJhdmF0YXIiOiIiLCJjb21wYW55IjoiNWMxZDRjMDExNGFjNGIwMDE2ODE4MDdmIiwicGhvbmVfdmVyaWZpZWQiOmZhbHNlLCJhY3RpdmUiOnRydWUsImxvY2FsZSI6ImVuX1VTIiwiZ3JvdXBzIjpbeyJsYWJlbF9jb2xvciI6IiM3MzdmZmEiLCJ1c2VycyI6W10sIl9pZCI6IjVjMWQ0YzAxMTRhYzRiMDAxNjgxODA4MCIsIm5hbWUiOiJBZG1pbiIsInBlcm1pc3Npb25zIjp7ImFkbWluIjp0cnVlfSwiY29tcGFueSI6IjVjMWQ0YzAxMTRhYzRiMDAxNjgxODA3ZiIsImNyZWF0ZWRBdCI6IjIwMTgtMTItMjFUMjA6MjQ6MzMuODkyWiIsInVwZGF0ZWRBdCI6IjIwMTgtMTItMjFUMjA6MjQ6MzMuODkyWiIsIl9fdiI6MH1dLCJsYW5ndWFnZV9vdmVycmlkZXMiOnt9fSwiaWF0IjoxNTU5OTM3OTc0fQ.DmWjursWGG4qKuW5sBUSC2XcMXk_vvUO2KGT4xMTNEc" // brand: "5bf4456457cf55001f0ebb0e",
    };
  },
  beforeMount() {
    this.getProduct();
  },
  methods: {
    async getProduct() {
      const resp = await axios.get(
        `https://parkcurity.herokuapp.com/products/${
          this.$route.params.id
        }?bbtoken=${this.token}`
      );
      this.product = resp.data.data;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
