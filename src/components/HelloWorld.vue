<template>
  <v-layout>
    <v-flex xs12 sm6 offset-sm3>
      <v-card>
        <v-container v-bind="{ [`grid-list-${size}`]: true }" fluid>
          <v-layout row wrap>
            <v-flex
              v-for="n in productItems"
              :key="n._id"
              xs4
              @mouseenter="hoverIn(n)"
              @mouseleave="hoverOut(n)"
              :class="{hovering: n.isHovering}"
            >
              <div class="overlay">
                <div class="p-name">{{n.name}}</div>
                <a :href="`/#/${n._id}`">
                  <v-btn color="info">Generate Barcode</v-btn>
                </a>
              </div>
              <v-card flat tile>
                <v-img :src="urlFromImage(n.techpack_images[0])" height="150px"></v-img>
              </v-card>
            </v-flex>
          </v-layout>
        </v-container>
      </v-card>
    </v-flex>
  </v-layout>
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
      size: "sm",
      msg: "My Product Barcodes",
      productName: null,
      productImage: null,
      images: [],
      gif: undefined,
      showCodes: false,
      products: [],
      productItems: [],
      bbRoute: "https://parkcurity.herokuapp.com/product",
      shopifyRoute: "https://parkcurity.herokuapp.com/shopify",
      slackRoute: "https://parkcurity.herokuapp.com/slack",
      token:
        "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7Il9pZCI6IjVjMWQ0YzAyYzExODEzMDAxNjUwYjBhZSIsIm5hbWUiOiJWaWN0b3JpYSBUZXN0Q28iLCJlbWFpbCI6InZpY3RvcmlhbGJyZXdlckBnbWFpbC5jb20iLCJwaG9uZSI6IjU0MTk2ODYxNTEiLCJhdmF0YXIiOiIiLCJjb21wYW55IjoiNWMxZDRjMDExNGFjNGIwMDE2ODE4MDdmIiwicGhvbmVfdmVyaWZpZWQiOmZhbHNlLCJhY3RpdmUiOnRydWUsImxvY2FsZSI6ImVuX1VTIiwiZ3JvdXBzIjpbeyJsYWJlbF9jb2xvciI6IiM3MzdmZmEiLCJ1c2VycyI6W10sIl9pZCI6IjVjMWQ0YzAxMTRhYzRiMDAxNjgxODA4MCIsIm5hbWUiOiJBZG1pbiIsInBlcm1pc3Npb25zIjp7ImFkbWluIjp0cnVlfSwiY29tcGFueSI6IjVjMWQ0YzAxMTRhYzRiMDAxNjgxODA3ZiIsImNyZWF0ZWRBdCI6IjIwMTgtMTItMjFUMjA6MjQ6MzMuODkyWiIsInVwZGF0ZWRBdCI6IjIwMTgtMTItMjFUMjA6MjQ6MzMuODkyWiIsIl9fdiI6MH1dLCJsYW5ndWFnZV9vdmVycmlkZXMiOnt9fSwiaWF0IjoxNTU5OTM3OTc0fQ.DmWjursWGG4qKuW5sBUSC2XcMXk_vvUO2KGT4xMTNEc", // brand: "5bf4456457cf55001f0ebb0e",
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
    this.getImages();
  },
  methods: {
    hoverIn(item) {
      item.isHovering = true;
    },
    hoverOut(item) {
      item.isHovering = false;
    },
    urlFromImage(imageItem) {
      if (imageItem && imageItem.image) {
        const id = imageItem.image;
        for (let image of this.images) {
          if (image._id == id) {
            return image.url;
          }
        }
      }
    },
    async getImages() {
      const resp = await axios.get(
        `https://parkcurity.herokuapp.com/images?bbtoken=${this.token}`
      );
      this.images = resp.data.data;
    },
    async getProducts() {
      const resp = await axios.get(
        `https://parkcurity.herokuapp.com/products?bbtoken=${this.token}`
      );
      for (let item of resp.data.data) {
        item.isHovering = false;
      }

      this.productItems = resp.data.data;
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
.v-card {
  background: whitesmoke;
}
.hovering {
  position: relative;
}
.overlay {
  display: none;
}
.hovering .overlay {
  height: 100%;
  display: block;
  width: 100%;
  background: black;
  opacity: 0.85;
  position: absolute;
  z-index: 2;
}
.p-name {
  color: white;
  margin-top: 20px;
  font-size: 30px;
}
.info {
  margin-top: 10px;
  background-color: #2196f3 !important;
  border-color: #2196f3 !important;
}
</style>
