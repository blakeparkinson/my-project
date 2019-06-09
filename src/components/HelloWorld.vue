<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button v-on:click="create">Create Codes</button>
    <ul v-if="showCodes">
      <li v-for="item in productItems" v-bind:key="item.name" class="items">
        <div>
          <span>
            <img v-bind:src="item.image" class="itmimg">
          </span>
          <span style="color:blue">{{item.name}}</span>
          <barcode v-bind:value="item.code">Show this if the rendering fails.</barcode>
        </div>
      </li>
    </ul>
    <ul>
      <li>
        <div>
          <img v-bind:src="gif">
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import Vue from "vue";

import axios from "axios";
import csvtojson from "csvtojson";
import VueBarcode from "vue-barcode";

export default {
  name: "HelloWorld",
  components: {
    barcode: VueBarcode
  },
  data() {
    return {
      msg: "My Product Barcodes",
      productName: null,
      productImage: null,
      gif: undefined,
      showCodes: false,
      products: [],
      productItems: [],
      bbRoute: "https://parkcurity.herokuapp.com/product",
      shopifyRoute: "https://parkcurity.herokuapp.com/shopify",
      slackRoute: "https://parkcurity.herokuapp.com/slack",
      token:
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7Il9pZCI6IjVjMWQ0YzAyYzExODEzMDAxNjUwYjBhZSIsIm5hbWUiOiJWaWN0b3JpYSBUZXN0Q28iLCJlbWFpbCI6InZpY3RvcmlhbGJyZXdlckBnbWFpbC5jb20iLCJwaG9uZSI6IjU0MTk2ODYxNTEiLCJhdmF0YXIiOiIiLCJjb21wYW55IjoiNWMxZDRjMDExNGFjNGIwMDE2ODE4MDdmIiwicGhvbmVfdmVyaWZpZWQiOmZhbHNlLCJhY3RpdmUiOnRydWUsImxvY2FsZSI6ImVuX1VTIiwiZ3JvdXBzIjpbeyJsYWJlbF9jb2xvciI6IiM3MzdmZmEiLCJ1c2VycyI6W10sIl9pZCI6IjVjMWQ0YzAxMTRhYzRiMDAxNjgxODA4MCIsIm5hbWUiOiJBZG1pbiIsInBlcm1pc3Npb25zIjp7ImFkbWluIjp0cnVlfSwiY29tcGFueSI6IjVjMWQ0YzAxMTRhYzRiMDAxNjgxODA3ZiIsImNyZWF0ZWRBdCI6IjIwMTgtMTItMjFUMjA6MjQ6MzMuODkyWiIsInVwZGF0ZWRBdCI6IjIwMTgtMTItMjFUMjA6MjQ6MzMuODkyWiIsIl9fdiI6MH1dLCJsYW5ndWFnZV9vdmVycmlkZXMiOnt9fSwiaWF0IjoxNTU5OTM3OTc0fQ.DmWjursWGG4qKuW5sBUSC2XcMXk_vvUO2KGT4xMTNEc",
      // brand: "5bf4456457cf55001f0ebb0e",
      pics: [
        "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRFzKEL_ITzrbBhjJt4baMP-3yaRUF0TMY3rRP7QPupBGVO3gES",
        "https://cnet3.cbsistatic.com/img/E_3uxOZR7-ryYQ1Uq6lEn1SFKTo=/970x0/2016/11/22/e4332ef1-a7fc-4cbc-ad05-5512e6d7d2ea/reindeer.jpg",
        "https://i0.wp.com/funkidsjokes.com/wp-content/uploads/2017/11/christmas-928328_640.png?fit=453%2C640&ssl=1",
        "https://i.pinimg.com/originals/f3/58/84/f35884360a76e5cc7cf5a0fdd1c9a75e.png",
        "https://secure.img2-fg.wfcdn.com/im/02472574/resize-h310-w310%5Ecompr-r85/5918/59186716/desmond-large-resin-shiny-reindeer-right-facing.jpg"
      ]
    };
  },
  beforeMount() {
    this.getProducts();
  },
  methods: {
    async getProducts() {
      const resp = await axios.get(
        `https://parkcurity.herokuapp.com/products?bbtoken=${this.token}`
      );
      this.productItems = resp.data.data;
      console.log(this.productItems);
    },
    async create(event) {
      // const csv = require("csvtojson");
      // const request = require("request");
      // csv()
      //   .fromStream(
      //     request.get(
      //       "http://insight.dev.schoolwires.com/HelpAssets/C2Assets/C2Files/C2ImportCalEventSample.csv"
      //     )
      //   )
      //   .subscribe(json => {
      //     console.log(json);
      //     return new Promise((resolve, reject) => {
      //       // long operation for each json e.g. transform / write into database.
      //     });
      //   });

      // this.reindeers.push({
      //   body: this.pics[Math.floor(Math.random() * this.pics.length)],
      //   head: this.productImage,
      //   name: this.productName,
      //   bbId: undefined
      // });
      // let integrations;
      // integrations = await this.doIntegrations();
      // this.products.push({
      //   name: this.productName,
      //   image: this.productImage,
      //   backboneId: integrations ? integrations.backboneId : undefined,
      //   shopId: integrations ? integrations.shopId : undefined
      // });
      // this.productName = null;
      // this.productImage = null;
      this.showCodes = true;
    },

    async doIntegrations() {
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
      const gifResponse = await axios.get(
        `https://parkcurity.herokuapp.com/giphy?q=${this.productName}`
      );
      this.gif = gifResponse.data.data[0].images.original.url;
      const response = await axios.post(this.bbRoute, criteria);

      const shopifyCriteria = {
        name: this.productName,
        image: this.productImage,
        backboneId: response.data.data._id
      };
      const response2 = await axios.post(this.shopifyRoute, shopifyCriteria);

      const slackCriteria = {
        attachments: [
          {
            image_url: this.productImage,
            title: "New Product Created",
            text: `Blake Parkinson added a new product: ${
              this.productName
            } , https://qa.backboneapp.co/products/${
              response.data.data._id
            }/summary`
          }
        ]
      };
      const response3 = await axios.post(this.slackRoute, slackCriteria);

      return { backboneId: response.data.data._id, shopId: response2.data.id };
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
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
.items {
  display: block;
}
.head {
  height: 50px;
  position: absolute;
  width: 50px;
  margin-top: 83px;
  margin-left: 50px;
}
.section {
  display: inline-flex;
}
.bod {
  width: 200px;
  height: 400px;
}
.itmimg {
  width: 100px;
}
a {
  color: #42b983;
}
#r-list li {
  display: block;
}
.spin {
  margin: 20px;
  width: 100px;
  height: 100px;
  background: #f00;
  -webkit-animation-name: spin;
  -webkit-animation-duration: 4000ms;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-timing-function: linear;
  -moz-animation-name: spin;
  -moz-animation-duration: 4000ms;
  -moz-animation-iteration-count: infinite;
  -moz-animation-timing-function: linear;
  -ms-animation-name: spin;
  -ms-animation-duration: 4000ms;
  -ms-animation-iteration-count: infinite;
  -ms-animation-timing-function: linear;

  animation-name: spin;
  animation-duration: 4000ms;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
}
@-ms-keyframes spin {
  from {
    -ms-transform: rotate(0deg);
  }
  to {
    -ms-transform: rotate(360deg);
  }
}
@-moz-keyframes spin {
  from {
    -moz-transform: rotate(0deg);
  }
  to {
    -moz-transform: rotate(360deg);
  }
}
@-webkit-keyframes spin {
  from {
    -webkit-transform: rotate(0deg);
  }
  to {
    -webkit-transform: rotate(360deg);
  }
}
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
