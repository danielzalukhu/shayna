<template>
  <div class="product">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="default_picture" alt="" />
                </div>
                <div class="product-thumbs" v-if="product_details.galleries.length > 0">
                  <carousel :dots="false" :nav="false" class="product-thumbs-track ps-slider">
                    <div 
                        v-for="picture in product_details.galleries" 
                        :key="picture.id" 
                        class="pt" 
                        @click="changeImage(picture.image_url)" 
                        :class="picture.image_url == default_picture ? 'active' : ''">
                          <img :src="picture.image_url" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ product_details.type }}</span>
                    <h3>{{ product_details.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p>
                      {{ product_details.description }}
                    </p>
                    <h4>Rp. {{ product_details.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart"><a href="#" class="primary-btn pd-cart">Add To Cart</a></router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProductShayna />

    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from "@/components/HeaderShayna.vue";
import FooterShayna from "@/components/FooterShayna.vue";
import RelatedProductShayna from "@/components/RelatedProductShayna.vue";
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "Product",
  components: {
    HeaderShayna,
    FooterShayna,
    RelatedProductShayna,
    carousel,
  },
  data() {
    return {
      default_picture: "",
      thumbs: ["img/mickey1.jpg", "img/mickey2.jpg", "img/mickey3.jpg", "img/mickey4.jpg"],
      product_details: [],
    };
  },
  methods: {
    changeImage(urlImage) {
      this.default_picture = urlImage;
    },
    // Fungsi untuk memanipulasi data setelah AXIOS berhasil menembak URL API
    getDataProduct(data) {
      //   1. replace object product_details dengan data dari API
      this.product_details = data;
      //   2. replace value untuk default_picture dengan parameter data yang udah kita manipulasi dengan
      //   data dari API
      this.default_picture = data.galleries[0].image_url;
    },
  },
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/v1/products", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((res) => this.getDataProduct(res.data.data))
      .catch((err) => console.log(err));
  },
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 14px;
}
</style>
