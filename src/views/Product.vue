<template>
  <div class="product w-100" style="background:#ccc">
    <NavbarView></NavbarView>
    <Loading :active="isLoading"></Loading>
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
            <p class="fs-4" style="text-decoration: line-through">原價:{{product.origin_price}}</p>
            <p class="fs-1" style="color:#f00">特價:{{product.price}}</p>
            <button
              type="button"
              @click="addToCart(product.id)"
              class="btn btn-base" style="color:#fff">加入購物車</button>
          </div>
        </div>
      </div>
      <div class="other mt-5 mb-5">
        <div class="title">
          <h2>你可能會喜歡:</h2>
        </div>
            <!-- <div class="toast-container" ref="toast">
              <div class="toast" role="alert" aria-live="assertive" aria-atomic="true">
                <div class="toast-header"> -->
                  <!-- <img src="..." class="rounded me-2" alt="..."> -->
                  <!-- <strong class="me-auto">Bootstrap</strong>
                  <small class="text-muted">just now</small>
                  <button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
                </div>
                <div class="toast-body">
                  See? Just like this.
                </div>
              </div>
            </div> -->
        <div class="container">
          <div class="row">
            <div class="products col-12 col-md-4" v-for="item in sameProduct" :key="item.id" >
                <div class="card mx-auto my-3" style="width: 100%;">
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
                        原:NT{{ item.origin_price }}
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
                        查看更多
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
                      ></i>加入購物車</button>
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
          // this.$refs.toast.show()

          this.isLoading = false
        })
        .catch((err) => {
          alert(err.data.message)
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
          alert(err.data.message)
        })
    },
    addToCart (id, qty = 1) {
      const url = `${process.env.VUE_APP_API}/apsi/${process.env.VUE_APP_PATH}/cart`
      this.loadingStatus.loadingItem = id
      const cart = {
        product_id: id,
        qty: qty
      }
      this.$http.post(url, { data: cart }).then((response) => {
        emitter.emit('cart')
        // alert(response.data.message)
        // this.getCart(id)
        this.$router.push('/carts')
        this.loadingStatus.loadingItem = ''
      }).catch((err) => {
        this.$httpMessageState(err.response, '錯誤訊息')
        alert(err)
      })
    }
    // getCart (id) {
    //   const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`
    //   this.isLoading = true
    //   this.$http
    //     .get(url)
    //     .then((response) => {
    //       this.cart = response.data.data.carts.product.filter(item => item.id === id)
    //       this.isLoading = false
    //     })
    //     .catch((err) => {
    //       alert(err.data.message)
    //     })
    // }
    // updateCart (data) {
    //   this.loadingStatus.loadingItem = data.id
    //   const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart/${data.id}`
    //   const cart = {
    //     product_id: data.id,
    //     qty: data.qty
    //   }
    //   this.$http
    //     .put(url, { data: cart })
    //     .then((response) => {
    //       alert(response.data.message)
    //       this.loadingStatus.loadingItem = ''
    //       this.getCart()
    //     })
    //     .catch((err) => {
    //       alert(err.data.message)
    //       this.loadingStatus.loadingItem = ''
    //     })
    // }

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
        // height: 15rem;
        width: 40%;
        background: #fff;
        border-radius:.5rem;
        // margin-left:-33px ;
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
      // height: 15rem;
      width: 100%;
      background: #fff;
      border-radius: 0 0 .5rem .5rem;
      // margin-left:-33px ;
      transform: translateX(0);
      margin: auto;
    }
  }
}
</style>
