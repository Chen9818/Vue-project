<template>
  <div class="carts">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <div class="d-md-block d-none" style="height:100vh;padding-top:130px">
        <div
          style="width: 80%"
          class="mx-auto mb-3 d-flex justify-content-between"
        >
          <h1>購物車</h1>
          <button
            type="button"
            class="btn btn-outline-secondary mb-3"
            @click="deleteAllCarts('open')"
            :disabled="
              cart.carts.length === 0 || loadingStatus.loadingItem === 'open'
            "
          >
            <i
              class="fas fa-spinner fa-pulse"
              v-if="loadingStatus.loadingItem === 'open'"
            ></i
            >清空購物車
          </button>
        </div>
      <table class="table align-middle mx-auto" style="width: 80%">
        <thead>
          <tr>
            <th>圖片</th>
            <th>商品名稱</th>
            <th>數量</th>
            <th>價格</th>
            <th>刪除</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in cart.carts" :key="item.id">
            <td style="width: 200px">
              <div
                style="
                  height: 100px;
                  background-size: cover;
                  background-position: center;
                "
                :style="{ backgroundImage: `url(${item.product.imageUrl})` }"
              ></div>
            </td>
            <td>
              {{ item.product.title }}
            </td>
            <td>
              <select
                name="productNum"
                class="form-select"
                style="width: 100%"
                v-model="item.qty"
                @change="updateCart(item)"
                :disabled="loadingStatus.loadingItem === item.id"
              >
                <option :value="num" v-for="num in 8" :key="`${num}${item.id}`">
                  {{ num }}
                </option>
              </select>
            </td>
            <td>
              {{ item.product.price * item.qty }}
            </td>
            <td>
              <i
                class="fas fa-trash-alt w-100"
                type="button"
                @click="removeCartItem(item)"
                :disabled="loadingStatus.loadingItem === item.id"
              >
              </i>
            </td>
          </tr>
        </tbody>
      </table>
      <div class="w-100">
        <div style="width: 80%" class="mx-auto d-flex justify-content-end">
          <h2 v-if="cart.final_total > 0">總金額:{{ cart.final_total }}</h2>
        </div>
      </div>
    </div>

    <!--RWD-->
    <ul class="d-md-none w-100 m-0" style="height:100vh;padding-top:130px">
      <div
        style="width: 80%; border-bottom: 1px solid #000"
        class="mx-auto mb-3 d-flex justify-content-between"
      >
        <h1>購物車</h1>
        <button
          type="button"
          class="btn btn-outline-secondary mb-3"
          @click="deleteAllCarts('open')"
          :disabled="
            cart.carts.length === 0 || loadingStatus.loadingItem === 'open'
          "
        >
          <i
            class="fas fa-spinner fa-pulse"
            v-if="loadingStatus.loadingItem === 'open'"
          ></i
          >清空購物車
        </button>
      </div>
      <li v-for="item in cart.carts" :key="item" class="mx-auto mb-3" style="width: 80%">
        <div class="card" style="width: 100%">
          <div class="card-body d-flex">
            <div style="width: 100px">
              <div
                style="
                  height: 100px;
                  background-size: cover;
                  background-position: center;
                "
                :style="{ backgroundImage: `url(${item.product.imageUrl})` }"
              ></div>
            </div>
            <div style="width: 400px">
              <div class="text-center mb-3">
                {{ item.product.title }}
              </div>
              <select
                name="productNum"
                class="form-select mx-auto mb-3"
                style="width: 80%"
                v-model="item.qty"
                @change="updateCart(item)"
                :disabled="loadingStatus.loadingItem === item.id"
              >
                <option :value="num" v-for="num in 8" :key="`${num}${item.id}`">
                  {{ num }}
                </option>
              </select>
              <div class="text-center">
                <span>金額:</span>
                {{ item.product.price * item.qty }}
                <i
                  class="fas fa-trash-alt px-5"
                  type="button"
                  @click="removeCartItem(item)"
                  :disabled="loadingStatus.loadingItem === item.id"
                >
                </i>
              </div>
            </div>
          </div>
        </div>
      </li>
      <div class="w-100">
        <div style="width: 80%" class="mx-auto d-flex justify-content-end">
          <h2 v-if="cart.final_total > 0">總金額:{{ cart.final_total }}</h2>
        </div>
      </div>
    </ul>

    <div class="w-50 mx-auto mt-5 mb-3" style="border-bottom: 1px solid #000;">
        <h1>訂購人資訊</h1>
    </div>
    <div class="mb-5 row justify-content-center">
      <v-form
        ref="form"
        class="col-md-6"
        v-slot="{ errors }"
        @submit="createOrder"
      >
        <div class="mb-3">
          <label for="name" class="form-label">收件人姓名<span class="px-1" style="color: #f00;">*</span></label>
          <v-field
            id="name"
            name="姓名"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['姓名'] }"
            placeholder="請輸入姓名"
            rules="required"
            v-model="form.user.name"
          ></v-field>
          <error-message
            name="姓名"
            class="invalid-feedback"
          ></error-message>
        </div>

        <div class="mb-3">
          <label for="email" class="form-label">收件人電子郵件<span class="px-1" style="color: #f00;">*</span></label>
          <v-field
            id="email"
            name="email"
            type="email"
            class="form-control"
            :class="{ 'is-invalid': errors['email'] }"
            placeholder="請輸入電子郵件"
            rules="email|required"
            v-model="form.user.email"
          ></v-field>
          <error-message
            name="email"
            class="invalid-feedback"
          ></error-message>
        </div>

        <div class="mb-3">
          <label for="tel" class="form-label">收件人電話<span class="px-1" style="color: #f00;">*</span></label>
          <v-field
            id="tel"
            name="電話"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['電話'] }"
            placeholder="請輸入電話"
            rules="required|min:8|max:10"
            v-model="form.user.tel"
          ></v-field>
          <error-message
            name="電話"
            class="invalid-feedback"
          ></error-message>
        </div>

        <div class="mb-3">
          <label for="address" class="form-label">收件人地址<span class="px-1" style="color: #f00;">*</span></label>
          <v-field
            id="address"
            name="地址"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['地址'] }"
            placeholder="請輸入地址"
            rules="required"
            v-model="form.user.address"
          ></v-field>
          <error-message
            name="地址"
            class="invalid-feedback"
          ></error-message>
        </div>

        <div class="mb-3">
          <label for="message" class="form-label">留言</label>
          <textarea
            name=""
            id="message"
            class="form-control"
            cols="30"
            rows="10"
            v-model="form.message"
          ></textarea>
        </div>
        <div class="text-end">
          <button type="submit" class="btn btn-danger"
          :disabled="Object.keys(errors).length > 0 || cart.carts.length === 0"
          >送出訂單</button>
        </div>
      </v-form>
    </div>

    <FooterView></FooterView>
  </div>
</template>

<script>
import FooterView from '@/components/FooterView.vue'
import NavbarView from '@/components/NavbarView.vue'

export default {
  name: 'Carts',
  data () {
    return {
      loadingStatus: {
        loadingItem: ''
      },
      cart: {
        carts: []
      },
      form: {
        user: {
          name: '',
          email: '',
          tel: '',
          address: ''
        },
        message: ''
      }
    }
  },
  components: {
    FooterView,
    NavbarView
  },
  mounted () {
    this.getCart()
  },
  methods: {
    getCart () {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`
      this.$http
        .get(url)
        .then((response) => {
          this.cart = response.data.data
        })
        .catch((err) => {
          alert(err.data.message)
        })
    },
    updateCart (data) {
      this.loadingStatus.loadingItem = data.id
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${data.id}`
      const cart = {
        product_id: data.product_id,
        qty: data.qty
      }
      this.$http
        .put(url, { data: cart })
        .then((response) => {
          alert(response.data.message)
          this.loadingStatus.loadingItem = ''
          this.getCart()
        })
        .catch((err) => {
          alert(err.data.message)
          this.loadingStatus.loadingItem = ''
        })
    },
    removeCartItem (data) {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${data.id}`
      this.loadingStatus.loadingItem = data.id
      this.$http
        .delete(url)
        .then((response) => {
          alert(response.data.message)
          this.loadingStatus.loadingItem = ''
          this.getCart()
        })
        .catch((err) => {
          alert(err.data.message)
        })
    },
    deleteAllCarts (data) {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/carts`
      this.loadingStatus.loadingItem = data
      this.$http
        .delete(url)
        .then((response) => {
          alert(response.data.message)
          this.loadingStatus.loadingItem = ''
          this.getCart()
        })
        .catch((err) => {
          alert(err.data.message)
        })
    },
    createOrder () {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/order`
      const order = this.form
      this.$http.post(url, { data: order }).then((response) => {
        alert(response.data.message)
        this.$refs.form.resetForm() // vee validate 的方法 form reset
        this.getCart()
      }).catch((err) => {
        alert(err.data.message)
      })
    }
  }
}
</script>
