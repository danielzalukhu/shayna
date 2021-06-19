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
                  <li @click="saveCart(product.id, product.name, product.price, product.galleries[0].image_url)" class="w-icon active">
                    <a href="javascript::void(0)" class="primary-btn pd-cart">
                      <i class="icon_bag_alt"></i>
                    </a>
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
      user_cart: [],
    };
  },
  methods: {
    saveCart(product_id, product_name, product_price, product_image) {
      const fetchData = {
        product_id: product_id,
        product_name: product_name,
        product_price: product_price,
        product_image: product_image,
      };

      this.user_cart.push(fetchData);
      const parsed = JSON.stringify(this.user_cart);
      localStorage.setItem("user_cart", parsed);

      this.$swal("Success add to cart");
    },
  },
  mounted() {
    if (localStorage.getItem("user_cart")) {
      try {
        this.user_cart = JSON.parse(localStorage.getItem("user_cart"));
      } catch (e) {
        localStorage.removeItem("user_cart");
      }
    }

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
