<template>
  <div class="products w-100" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title="MainTitle"></MainImage>
    <ul class="filter d-flex justify-content-center pt-5 d-none d-xl-flex">
      <li><button type="button" class="btn btn-base" style="color:#fff;font-size:30px" @click="getProducts()">全部</button></li>
      <li><button type="button" class="btn btn-base" style="color:#fff;font-size:30px" @click="getFilter('乳膠枕')">乳膠枕</button></li>
      <li><button type="button" class="btn btn-base" style="color:#fff;font-size:30px" @click="getFilter('絲絨枕')">絲絨枕</button></li>
      <li><button type="button" class="btn btn-base" style="color:#fff;font-size:30px" @click="getFilter('機能枕')">機能枕</button></li>
      <li><button type="button" class="btn btn-base" style="color:#fff;font-size:30px" @click="getFilter('兒童枕')">兒童枕</button></li>
    </ul>
    <div class="dropdown d-block d-xl-none d-flex justify-content-center mt-5">
      <button class="btn btn-base dropdown-toggle fs-3" style="color:#fff;width:80%" type="button" id="dropdownMenuButton1" data-bs-toggle="dropdown" aria-expanded="false">
        {{filterTitle}}
      </button>
      <ul class="dropdown-menu text-center" aria-labelledby="dropdownMenuButton1" style="width:80%">
        <li class="dropdown-item fs-3" @click="getProducts">全部</li>
        <li class="dropdown-item fs-3" @click="getFilter('乳膠枕')">パイプ枕</li>
        <li class="dropdown-item fs-3" @click="getFilter('絲絨枕')">わた枕</li>
        <li class="dropdown-item fs-3" @click="getFilter('機能枕')">機能枕</li>
        <li class="dropdown-item fs-3" @click="getFilter('兒童枕')">児童枕</li>
      </ul>
    </div>

      <div class="container">
        <div class="row">
          <div class="products col-12 col-md-4 d-flex flex-wrap" v-for="item in products" :key="item.id">
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
                  @click="getProduct(item.id)"
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
      <PaginationView
      :pages="pagination"
      @emit-pages="getProducts"
      class="d-flex justify-content-center"
      ></PaginationView>
    <FooterView></FooterView>
  </div>
</template>

<script>
import FooterView from '@/components/FooterView.vue'
import NavbarView from '@/components/NavbarView.vue'
import MainImage from '@/components/MainImage.vue'
import PaginationView from '@/components/PaginationView.vue'
import emitter from '../utility/emitter'

export default {
  name: 'Products',
  data () {
    return {
      products: [],
      filterProducts: [],
      filterTitle: '',
      loadingStatus: {
        loadingItem: ''
      },
      isLoading: false,
      product: {},
      pagination: {},
      MainTitle: '商品列表'
    }
  },
  components: {
    FooterView,
    NavbarView,
    PaginationView,
    MainImage
  },
  mounted () {
    this.getProducts()
  },
  methods: {
    getProducts (id = 1) {
      this.isLoading = true
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products?page=${id}`
      this.$http
        .get(url)
        .then((response) => {
          const { products, pagination } = response.data
          this.products = products
          this.pagination = pagination
          this.filterTitle = '全部'
          this.isLoading = false
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    },
    getProduct (id) {
      this.$router.push(`/product/${id}`)
    },
    getFilter (e) {
      this.isLoading = true
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`
      this.$http
        .get(url)
        .then((response) => {
          this.filterProducts = response.data.products
          this.filterTitle = e
          this.products = this.filterProducts.filter(item => item.category === e)
          this.isLoading = false
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    },
    addToCart (id, qty = 1) {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`
      this.loadingStatus.loadingItem = id
      const cart = {
        product_id: id,
        qty
      }
      this.$http.post(url, { data: cart }).then((response) => {
        emitter.emit('cart')
        this.$httpMessageState(response, '加入購物車')
        this.loadingStatus.loadingItem = ''
      }).catch((err) => {
        this.$httpMessageState(err.response, '加入購物車')
      })
    }
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";
  .filter{
    width: 50%;
    margin: auto;
    li{
      margin:0 2px 0 2px;
    }
  }
  .card {
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

@media (max-width:600px){
  .products{
    width:70%;
    margin:auto;
  }
}
</style>
