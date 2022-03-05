<template>
  <div class="products" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <div class="main-img">
      <h3>商品列表</h3>
    </div>
    <ul class="d-flex">
      <li><button type="button" class="btn btn-primary">Primary</button></li>
      <li><button type="button" class="btn btn-primary">Primary</button></li>
      <li><button type="button" class="btn btn-primary">Primary</button></li>
      <li><button type="button" class="btn btn-primary">Primary</button></li>
      <li><button type="button" class="btn btn-primary">Primary</button></li>
      <li><button type="button" class="btn btn-primary">Primary</button></li>
    </ul>
      <div class="container" style="padding: top 1.5rem;">
        <div class="row ">
          <div class="col-6 col-md-4 d-flex flex-wrap" v-for="item in products" :key="item.id">
            <div class="card mx-auto my-5" style="width: 100%;">
              <img
                :src="item.imageUrl"
                class="card-img-top"
                :alt="item.title"
              />
              <div class="card-body">
                <div class="d-flex">
                  <h4 class="card-title">
                    {{ item.title }}
                  <span class="badge bg-primary ms-2">{{ item.category }}</span>
                  </h4>
                </div>
                <p class="card-text"/>
                  <p class="text-decoration-line-through">
                    原價:NT{{ item.origin_price }}
                  </p>
                  特價:NT{{ item.price }}
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
      <PaginationView
      :pages="pagination"
      @emit-pages="getProducts"
      ></PaginationView>
    <FooterView></FooterView>
  </div>
</template>

<script>
import FooterView from '@/components/FooterView.vue'
import NavbarView from '@/components/NavbarView.vue'
import PaginationView from '@/components/PaginationView.vue'

export default {
  name: 'Products',
  data () {
    return {
      products: [],
      loadingStatus: {
        loadingItem: ''
      },
      isLoading: false,
      product: {},
      pagination: {}
    }
  },
  components: {
    FooterView,
    NavbarView,
    PaginationView
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
          this.isLoading = false
        })
        .catch((err) => {
          alert(err.data.message)
        })
    },
    getProduct (id) {
      this.$router.push(`/product/${id}`)
    },
    addToCart (id, qty = 1) {
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`
      this.loadingStatus.loadingItem = id
      const cart = {
        product_id: id,
        qty
      }
      this.$http.post(url, { data: cart }).then((response) => {
        alert(response.data.message)
        this.loadingStatus.loadingItem = ''
      }).catch((err) => {
        alert(err.data.message)
      })
    }
  }
}
</script>

<style lang="scss" scoped>
  .main-img{
  background: url(../assets/pic/main-page/main-img.png);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  background-attachment: fixed;
  width: 100%;
  height: 50vh;
  z-index: 2;
  // padding-top: 115px;
  h3{
    font-size: 150px;
    // text-align: center;
    // display: flex;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
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
</style>
