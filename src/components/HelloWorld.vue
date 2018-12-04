<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <input v-model="productName" placeholder="Product Name">
    <input v-model="productImage" placeholder="Product Image URL">
    <button v-on:click="create">Create Product</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      productName: null,
      productImage: null
    }
  },
  methods: {
    create (event) {
      console.log(this.productName)
    }
  }
    
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
