<template>
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel class="product-slider" :items="3" :nav="false" :autoplay="false" :dots="false">
            <div class="product-item" v-for="product in products" v-bind:key="product.id">
              <div class="pi-pic">
                <img v-bind:src="product.galleries[0].image_url" alt="" />
                <ul>
                  <li class="w-icon active">
                    <router-link v-bind:to="'/product/' + product.id"><i class="icon_bag_alt"></i></router-link>
                  </li>
                  <li class="quick-view">
                    <router-link v-bind:to="'/product/' + product.id">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ product.type }}</div>
                <h5>{{ product.name }}</h5>
                <div class="product-price">
                  Rp. {{ product.price }}
                  <!-- <span>$115.00</span> -->
                </div>
              </div>
            </div>
          </carousel>
        </div>
        <div class="col-lg-12 mt-5" v-else>
          <h5>Product Not Available</h5>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "WomenShayna",
  components: {
    carousel,
  },
  data() {
    return {
      products: [],
    };
  },
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/v1/products")
      .then((res) => (this.products = res.data.data))
      .catch((err) => console.log(err));
  },
};
</script>

<style scoped>
.product-item {
  margin-right: 25px;
}
</style>
