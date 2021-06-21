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
                      <tr v-for="cart in user_cart" :key="cart.product_id">
                        <td class="cart-pic first-row">
                          <img class="img-cart" :src="cart.product_image" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ cart.product_name }}</h5>
                        </td>
                        <td class="p-price first-row">{{ cart.product_price }}</td>
                        <td>
                          <div class="quantity">
                            <button class="btn minus-btn disabled" type="button" @click="decrement()">-</button>
                            <input type="text" v-model="input_quantity" min="1" />
                            <button class="btn plus-btn" type="button" @click="increment()">+</button>
                          </div>
                        </td>
                        <td class="p-price first-row" v-bind:sub_total="cart.product_price * input_quantity">{{ cart.product_price * input_quantity }}</td>
                        <td class="delete-item" @click="removeCartItem(user_cart.product_id)">
                          <a href="#">
                            <i class="material-icons">delete</i>
                          </a>
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
                      <input 
                        type="text" 
                        class="form-control" 
                        id="namaLengkap" 
                        aria-describedby="namaHelp" 
                        placeholder="Customer Name" 
                        v-model="customer_data.name"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">Email Address</label>
                      <input 
                        type="email"  
                        class="form-control" 
                        id="emailAddress" 
                        aria-describedby="emailHelp" 
                        placeholder="Email" 
                        v-model="customer_data.email"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">No. HP</label>
                      <input 
                        type="text" 
                        class="form-control" 
                        id="noHP" 
                        aria-describedby="noHPHelp" 
                        placeholder="Phone Number" 
                        v-model="customer_data.number"
                      />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Address</label>
                      <textarea 
                        class="form-control" 
                        id="alamatLengkap" 
                        rows="4"
                        v-model="customer_data.address">
                      </textarea>
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
                    <li class=" subtotal mt-3">
                      Total Biaya (Rp) <span> {{ totalPrice() }}</span>
                    </li>
                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                    <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                  </ul>
                  <!-- <router-link to="/success"> -->
                    <a href="#" @click="checkout()" class="proceed-btn">I ALREADY PAID</a>
                  <!-- </router-link> -->
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
import axios from "axios";

export default {
  name: "ShoppingCart",
  components: {
    HeaderShayna,
  },
  data() {
    return {
      user_cart: [],
      input_quantity: "1",
      customer_data: {
        name: "",
        email: "",
        number: "",
        address: "",
      },
    };
  },
  methods: {
    removeCartItem(idx) {
      // Cari tau id dari si item yang akan dihapus
      let userCartStorage = JSON.parse(localStorage.getItem("user_cart"));
      let itemInUserCartStorage = userCartStorage.map(itemInUserCartStorage => itemInUserCartStorage.product_id);

      // Cocokin id item yang mau dihapus (artinya id product yg diklik oleh user) dgn yang ada di localStorage
      let index = itemInUserCartStorage.findIndex(product_id => product_id == idx);
      this.user_cart.splice(index, 1);

      const parsed = JSON.stringify(this.user_cart);
      localStorage.setItem("user_cart", parsed);
      window.location.reload();
    },
    decrement() {
      this.input_quantity--;

      if (this.input_quantity < 0) {
        this.$swal.fire({
          title: "Error!",
          text: "Minimun quantity is: 1",
          icon: "error",
          confirmButtonText: "Understand",
        });
        this.input_quantity = 0;
      }
    },
    increment() {
      this.input_quantity++;
    },
    totalPrice() {
      return this.user_cart.reduce(function(items, value) {
        return items + value.product_price;
      }, 0);
    },
    checkout() {
      let products =this.user_cart.map(function(product){
        return {
          product_id: product.product_id,
          qty: 5,
          subtotal: product.product_price 
        }
      })
      // console.log(products);

      let sendProducts = {
        name: this.customer_data.name,
        email: this.customer_data.email,
        number: this.customer_data.number,
        address: this.customer_data.address,
        transaction_total: this.totalPrice(),
        transaction_status: "pending",
        transaction_details: products
      };
      // console.log(sendProducts);

      axios 
        .post(
          "http://127.0.0.1:8000/api/v1/checkout", 
          sendProducts
        )
        .then(() => this.$router.push('success'))
        // es-lint-disable-next-line no-console
        .catch(err => console.log(err));

        this.user_cart.length = [];
      const parsed = JSON.stringify(this.user_cart);
      localStorage.setItem("user_cart", parsed);
    }
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
.img-cart {
  width: 100px;
  height: 100px;
}

.quantity {
  display: flex;
  justify-content: center;
  /* width: 10px; */
}

.quantity button {
  width: 30px;
  height: 45px;
  border: 1px solid #000;
  color: #000;
  border-radius: 0;
  background: #fff;
}

.quantity input {
  border: none;
  border-top: 1px solid #000;
  border-bottom: 1px solid #000;
  text-align: center;
  width: 100px;
  font-size: 20px;
  color: #000;
  font-weight: 300;
}

.total-price {
  text-align: center;
  font-size: 30px;
  color: #fff;
}
</style>
