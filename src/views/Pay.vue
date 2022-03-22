<template>
  <div class="a" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title='title'></MainImage>
    <div class="pay w-100 py-5" style="display:flex">
      <div class="d-flex justify-content-center" style="width:60%">
        <div class="" style="width:80%">
          <div class="text">
            <h2>最後一步，完成訂單</h2>
          </div>
          <div class="img w-100">
            <img src="@/assets/pic/main-page/payDone.png" class="w-100" alt="">
          </div>
        </div>
      </div>
      <div class="px-4 fs-4" style="width:40%">
        <div class="" style="width:80%">
          <h2>訂單明細</h2>
          <div class="p-2 d-flex justify-content-between" style="border-bottom:1px solid #fff">
              <div class="">姓名</div>
              <div>{{order.name}}</div>
          </div>
          <div class="p-2 d-flex justify-content-between" style="border-bottom:1px solid #fff">
              <div class="">電話</div>
              <div>{{order.tel}}</div>
          </div>
          <div class="p-2 d-flex justify-content-between" style="border-bottom:1px solid #fff">
              <div class="">地址</div>
              <div>{{order.address}}</div>
          </div>
          <div class="p-2 d-flex justify-content-between" style="border-bottom:1px solid #fff">
              <div class="">email</div>
              <div>{{order.email}}</div>
          </div>
          <div class="p-2 d-flex justify-content-between" style="border-bottom:1px solid #fff">
              <div class="">付款方式</div>
              <div>{{payMethod}}</div>
          </div>
          <div class="p-2 d-flex justify-content-between" style="border-bottom:1px solid #fff;color:#ff0000">
              <div class="">總計</div>
              <div>{{total}}</div>
          </div>
          <div class="text-end mt-5">
            <button type="button" @click='payDone' class="btn btn-base fs-4" style="color:#fff">完成訂單</button>
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
      title: '確認付款',
      isLoading: false,
      order: {
        name: '',
        address: '',
        tel: '',
        email: ''
      },
      total: '',
      id: '',
      payMethod: ''
    }
  },
  components: {
    NavbarView,
    FooterView,
    MainImage
  },
  methods: {
    getPayMethod () {
      const { id } = this.$route.params
      this.payMethod = id
    },
    getOrder () {
      this.isLoading = true
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/orders`
      this.$http
        .get(url)
        .then((response) => {
          const { user, total, id } = response.data.orders[0]
          this.order = user
          this.total = total
          this.id = id
          this.isLoading = false
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    },
    payDone () {
      this.isLoading = true
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/pay/${this.id}`
      this.$http
        .post(url)
        .then((response) => {
          this.$router.push('/payDone')
          this.$httpMessageState(response, '付款')
          this.isLoading = false
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    }
  },
  mounted () {
    this.getPayMethod()
    this.getOrder()
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/base/all.scss';

@media (max-width: 750px) {
  .a .pay{
    display: block;
  }
}
</style>
