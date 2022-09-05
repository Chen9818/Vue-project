<template>
  <div class="product w-100" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <!-- 特定商品部分 -->
    <div class="container" style="padding-top:13rem">
      <div class="productShow">
        <div class="img">
          <img :src="product.imageUrl" :alt="product.title">
        </div>
        <div class="txt p-3">
          <h1 class="mb-3 pb-2" style="border-bottom:2px solid #000">{{product.title}}</h1>
          <h2>「{{product.description}}」</h2>
          <p>尺寸:{{product.content}}</p>
          <div class="d-flex justify-content-between">
            <p class="fs-4" style="text-decoration: line-through">原:{{product.origin_price}}</p>
            <p class="fs-1" style="color:#f00">特:{{product.price}}</p>
            <button
              type="button"
              @click="addToCart(product.id)"
              class="btn btn-base" style="color:#fff"
              :disabled="cart[0]?.qty>7"
              >カートに入れる</button>
          </div>
        </div>
      </div>

      <!-- 相關商品部分 -->
      <div class="other mt-5 mb-5">
        <div class="title">
          <h2>関連する商品:</h2>
        </div>
        <div class="container">
          <div class="row">
            <div class="products col-12 col-md-4" v-for="item in sameProduct" :key="item.id" >
                <div class="card mx-auto my-3" style="width: 100%;height:100%">
                  <img
                    :src="item.imageUrl"
                    class="card-img-top"
                    :alt="item.title"
                  />
                  <div class="card-body">
                    <div class="d-flex">
                      <h4 class="card-title">
                        {{ item.title }}
                      <span class="badge bg-base ms-2">{{ item.category }}</span>
                      </h4>
                    </div>
                      <div class="card-text d-flex justify-content-between" style="font-size:1.2rem">
                      <p class="text-decoration-line-through">
                        元:NT{{ item.origin_price }}
                      </p>
                      <p style="color:#ff0000">
                        特:NT{{ item.price }}
                      </p>
                      </div>
                    <div class="d-flex justify-content-between">
                      <button
                      type="button"
                      class="btn btn-outline-secondary"
                      @click="getToProduct(item.id)"
                      :disabled="loadingStatus.loadingItem === item.id || !item.is_enabled"
                      >
                        <i
                        class="fas fa-spinner fa-pulse"
                        v-if="loadingStatus.loadingItem === item.id"
                        ></i>
                        MORE
                      </button>
                      <button
                      type="button"
                      class="btn btn-outline-danger"
                      @click="addToCart(item.id)"
                      :disabled="loadingStatus.loadingItem === item.id || !item.is_enabled"
                      >
                        <i
                        class="fas fa-spinner fa-pulse"
                        v-if="loadingStatus.loadingItem === item.id"
                      ></i>コートに入れる</button>
                    </div>
                  </div>
                </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <FooterView></FooterView>
  </div>
</template>

<script>
import NavbarView from '@/components/NavbarView.vue'
import FooterView from '@/components/FooterView.vue'
import emitter from '../utility/emitter'

export default {
  data () {
    return {
      product: {
        qty: ''
      },
      isLoading: false,
      products: [],
      sameProduct: [],
      loadingStatus: {
        loadingItem: ''
      },
      cart: []
    }
  },
  components: {
    NavbarView,
    FooterView
  },
  methods: {
    getProduct () {
      const { id } = this.$route.params // 取出params
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/product/${id}`
      this.isLoading = true
      this.$http
        .get(url)
        .then((response) => {
          this.product = response.data.product
          this.getProducts()
          this.getCart(id)
          this.isLoading = false
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    },
    getToProduct (id) {
      this.$router.push(`/product/${id}`).then(() => this.getProduct())
    },
    getProducts () {
      this.isLoading = true
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`
      this.$http
        .get(url)
        .then((response) => {
          this.products = response.data.products
          this.sameProduct = this.products.filter((item) => {
            return item.category === this.product.category && item.title !== this.product.title
          })
          this.isLoading = false
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    },
    addToCart (id, qty = 1) {
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`
      this.loadingStatus.loadingItem = id
      const cart = {
        product_id: id,
        qty: qty
      }
      this.$http.post(url, { data: cart }).then((response) => {
        emitter.emit('cart')
        this.loadingStatus.loadingItem = ''
        this.getCart(id)
        this.$httpMessageState(response, 'カートに入れる')
        this.$router.push('/carts')
      }).catch((err) => {
        this.$httpMessageState(err.response, 'カートに入れる')
      })
    },
    getCart (id) {
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`
      this.isLoading = true
      this.$http
        .get(url)
        .then((response) => {
          this.cart = response.data.data.carts.filter(item => item.product_id === id)
          this.isLoading = false
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    }
  },
  mounted () {
    this.getProduct()
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";
  .container{
    .productShow {
    display: flex;
    justify-content: center;
    box-sizing: border-box;
      .img{
        width: 60%;
        height:30rem;
        img{
          width: 100%;
          height: 100%;
          background-position: center center;
          background-size: cover;
        }
      }
      .txt{
        width: 40%;
        background: #fff;
        border-radius:.5rem;
        transform: translateX(-33px);
        margin: auto;
      }
    }
    .other .card {
    opacity: 0.8;
      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
      &:hover {
        opacity: 1;
        img {
          border: 10px solid #fff;
        }
      }
    }
  }

  @media (max-width:780px){
  .container .productShow {
    display: block;
    .img{
      width: 100%;
      height:30rem;
    }
    .txt{
      width: 100%;
      background: #fff;
      border-radius: 0 0 .5rem .5rem;
      transform: translateX(0);
      margin: auto;
    }
  }
}
</style>
