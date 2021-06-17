<template>
  <div class="shopping">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Shopping Cart</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
      <div class="container">
        <div class="row">
          <div class="col-lg-8">
            <div class="row">
              <div class="col-lg-12">
                <div class="cart-table">
                  <table>
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product</th>
                        <th>Price <b>(Rp)</b></th>
                        <th>Qty</th>
                        <th>Subtotal <b>(Rp)</b></th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="cart in user_cart" :key="cart.id">
                        <td class="cart-pic first-row">
                          <img class="img-cart" :src="cart.product_image" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ cart.product_name }}</h5>
                        </td>
                        <td class="p-price first-row">{{ cart.product_price }}</td>
                        <!-- <td class="p-quantity first-row">2</td> -->
                        <td class="p-qty first-row">
                          <form>
                            <div class="col-lg-2">
                              <div class="input-group">
                                <span class="input-group-btn">
                                  <button type="button" class="quantity-left-minus btn btn-danger btn-number"  data-type="minus" data-field="">
                                    <span class="glyphicon glyphicon-minus"></span>
                                  </button>
                                </span>
                                <input type="text" id="quantity" name="quantity" class="form-control input-number" value="10" min="1" max="100">
                                <span class="input-group-btn">
                                    <button type="button" class="quantity-right-plus btn btn-success btn-number" data-type="plus" data-field="">
                                        <span class="glyphicon glyphicon-plus"></span>
                                    </button>
                                </span>
                              </div>
                            </div>
                          </form>
                        </td> 
                        <td class="p-sub-total first-row">300000</td>
                        <td class="delete-item" @click="removeCartItem(user_cart.index)">
                          <a href="#"
                            ><i class="material-icons">
                              close
                            </i></a
                          >
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="col-lg-8">
                <h4 class="mb-4 text-left">
                  Informasi Pembeli:
                </h4>
                <div class="user-checkout text-left">
                  <form>
                    <div class="form-group">
                      <label for="namaLengkap">Nama lengkap</label>
                      <input type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama" />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">Email Address</label>
                      <input type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email" />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">No. HP</label>
                      <input type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP" />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Alamat Lengkap</label>
                      <textarea class="form-control" id="alamatLengkap" rows="3"></textarea>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-4">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout text-left">
                  <ul>
                    <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                    <li class="subtotal mt-3">Subtotal (Rp) <span>240.00</span></li>
                    <li class="subtotal mt-3">Total Biaya (Rp) <span>440.00</span></li>
                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                    <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                  </ul>
                  <router-link to="/success"><a href="#" class="proceed-btn">I ALREADY PAID</a></router-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Shopping Cart Section End -->
  </div>
</template>

<script>
import HeaderShayna from "@/components/HeaderShayna.vue";

export default {
  name: "ShoppingCart",
  components: {
    HeaderShayna,
  },
  data() {
    return {
      user_cart: []
    }
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

// eslint-disable-next-line no-undef
$(document).ready(function(){
   // eslint-disable-next-line no-undef
  $('.quantity-right-plus').click(function(e){
    e.preventDefault();
    // eslint-disable-next-line no-undef
    var quantity = parseInt($('#quantity').val());
    // eslint-disable-next-line no-undef
    $('#quantity').val(quantity + 1);
  });

  // eslint-disable-next-line no-undef
  $('.quantity-left-minus').click(function(e){
    e.preventDefault();
    // eslint-disable-next-line no-undef
    var quantity = parseInt($('#quantity').val());
        
    if(quantity>0){
      // eslint-disable-next-line no-undef
      $('#quantity').val(quantity - 1);
    }
  });    
});
</script>

<style scoped>
.img-cart {
  width: 200px;
  height: 125px;
}
</style>
