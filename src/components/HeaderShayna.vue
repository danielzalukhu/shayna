<template>
  <header class="header-section">
    <div class="header-top">
      <div class="container">
        <div class="ht-left">
          <div class="mail-service"><i class=" fa fa-envelope"></i> clothing.shayna@gmail.com</div>
          <div class="phone-service"><i class=" fa fa-phone"></i> +62 857 3377 0050</div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="inner-header">
        <div class="row">
          <div class="col-lg-2 col-md-2">
            <div class="logo">
              <router-link to="/">
                <img src="img/logo_website_shayna.png" alt="" />
              </router-link>
            </div>
          </div>
          <div class="col-lg-7 col-md-7"></div>
          <div class="col-lg-3 text-right col-md-3">
            <ul class="nav-right">
              <li class="cart-icon">
                Keranjang Belanja &nbsp;
                <a href="#">
                  <i class="icon_bag_alt"></i>
                  <span>{{ user_cart.length }}</span>
                </a>
                <div class="cart-hover">
                  <div class="select-items">
                    <table>
                      <tbody v-if="user_cart.length > 0">
                        <tr v-for="cart in user_cart" :key="cart.id">
                          <td class="si-pic">
                            <img class="photo-item" :src="cart.product_image" alt="" />
                          </td>
                          <td class="si-text">
                            <div class="product-selected">
                              <p>Rp. {{ cart.product_price }}</p>
                              <h6>{{ cart.product_name }}</h6>
                            </div>
                          </td>
                          <td @click="removeCartItem(user_cart.index)" class="si-close">
                            <i class="ti-close"></i>
                          </td>
                        </tr>
                      </tbody>
                      <tbody v-else>
                        <tr>
                          <td>Not Available Items</td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                  <div class="select-total">
                    <span>total:</span>
                    <h5>Rp. SUBTOTAL</h5>
                  </div>
                  <div class="select-button">
                    <a href="#" class="primary-btn view-card">VIEW CARD</a>
                    <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "HeaderShayna",
  data() {
    return {
      user_cart: [],
    };
  },
  methods: {
    removeCartItem(index) {
      this.user_cart.splice(index, 1);
      const parsed = JSON.stringify(this.user_cart);
      localStorage.setItem("user_cart", parsed);
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
  },
};
</script>

<style scoped>
.photo-item {
  width: 250px;
  height: 140px;
}
</style>
