<template>
  <div class="product w-100" style="background:#ccc">
    <NavbarView></NavbarView>
    <Loading :active="isLoading"></Loading>
    <div class="container" style="padding-top:13rem">
      <div class="productShow">
        <div class="img">
          <img :src="product.imageUrl" :alt="product.title">
        </div>
        <div class="txt">
          <h1>{{product.title}}</h1>
          <h2>{{product.description}}</h2>
          <p>尺寸:{{product.content}}</p>
          <p>原價:{{product.origin_price}}</p>
          <p>特價:{{product.price}}</p>
          <div class="addToCart d-flex">
              <!-- <select
                name="productNum"
                class="form-select"
                style="width: 100%"
                v-model="product.qty"
                :disabled="loadingStatus.loadingItem === product.id"
              >
                <option  :value="num" v-for="num in 8" :key="num">
                  {{num}}
                </option>
              </select> -->
              <button
              type="button"
              @click="addToCart(product.id)"
              class="btn btn-primary">加入購物車</button>
              <button
              type="button"
              class="btn btn-primary">已達購買上限</button>
          </div>
        </div>
      </div>
      <div class="other">
        <div class="title">
          <h2>相關商品{{cart}}</h2>
        </div>

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
  </div>
</template>

<script>
import NavbarView from '@/components/NavbarView.vue'
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
    NavbarView
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

          this.isLoading = false
        })
        .catch((err) => {
          alert(err.data.message)
        })
    },
    getToProduct (id) {
      // this.$router.push('/products')
      this.$router.push(`/product/${id}`)
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
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`
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
        alert(err.data.message)
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
      // margin-left:-33px ;
      transform: translateX(0);
      margin: auto;
    }
  }
}
</style>
