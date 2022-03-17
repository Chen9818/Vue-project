<template>
<div class="payment" style="background:#ccc;">
<NavbarView></NavbarView>
<MainImage :title='title'></MainImage>
<div class="mt-5 w-100 p-3 d-flex">
  <div class="w-50">
    <h3>購物清單</h3>
    <table class="table">
      <thead>
        <tr>
          <th scope="col"></th>
          <th scope="col">名稱</th>
          <th scope="col">單價</th>
          <th scope="col">數量</th>
          <th scope="col">價格</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item,id) in carts.carts" :key='id'>
          <th scope="row" style="width:6rem;height:6rem">
            <img :src="item.product.imageUrl" :alt="item.product.title" style="width:100%">
          </th>
          <td>{{item.product.title}}</td>
          <td>{{item.product.price}}</td>
          <td>{{item.qty}}</td>
          <td>{{item.final_total}}</td>
        </tr>
      </tbody>
    </table>
    <h3 class="d-flex justify-content-end">總金額:{{carts.total}}</h3>
  </div>
  <div class="w-50">
    <div class="couponPart text-center">
      <div class="coupon">
        <div class="couponImage fs-3">
          <div class="couponTxt">
            夏日特賣
            <br>折扣碼:777
          </div>
        </div>
        <div class="couponNum">
          <input type="text" placeholder="輸入折扣碼:777" class="w-50 fs-4">
        </div>
      </div>
    </div>
    <div class="payment text-center mb-5 fs-5 d-flex align-items-center justify-content-center">
      <div class="">
        <label for="pay">付款方式:</label>
        <select id="pay">
          <option value="">信用卡</option>
          <option value="">ATM</option>
          <option value="">超商繳費</option>
          <option value="">ApplePay</option>
          <option value="">LinePay</option>
        </select>
      </div>
      <div class="pay text-center my-3 fs-5 ml-3">
        <button type="button" class="btn btn-danger" style="color:#fff">
          完成結帳
        </button>
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
import MainImage from '@/components/MainImage.vue'

export default {
  data () {
    return {
      title: '付款方式',
      carts: []
    }
  },
  components: {
    NavbarView,
    FooterView,
    MainImage
  },
  mounted () {
    this.getCart()
  },
  methods: {
    getCart () {
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`
      this.$http(url)
        .then((response) => {
          this.carts = response.data.data
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/base/all.scss';
.couponImage{
  background: url('../assets/pic/main-page/summer.png');
  background-position: center center;
  background-repeat: no-repeat;
  background-size:50% 100%;
  text-align: center;
  height: 20rem;
  text-align: center;
  color:#fff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  .couponTxt{
    background: $base-color;
    text-align: center;
    margin: auto;
    width: 30%;
    border-radius: 1rem;
  }
}

</style>
