<template>
<div class="payment" style="background:#ccc;">
<NavbarView></NavbarView>
<MainImage :title='title'></MainImage>
<div class="mt-5 w-100 p-3 d-flex">
  <div class="w-50">
        <div class="mb-5 row justify-content-center w-100 mx-auto">
      <Form
        ref="form"
        class="col-md-6"
        v-slot="{ errors }"
        @submit="createOrder"
      >
        <div class="mb-3">
          <label for="name" class="form-label">收件人姓名<span class="px-1" style="color: #f00;">*</span></label>
          <Field
            id="name"
            name="姓名"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['姓名'] }"
            placeholder="請輸入姓名"
            rules="required"
            v-model="form.user.name"
          ></Field>
          <ErrorMessage
            name="姓名"
            class="invalid-feedback"
          ></ErrorMessage>
        </div>

        <div class="mb-3">
          <label for="email" class="form-label">收件人電子郵件<span class="px-1" style="color: #f00;">*</span></label>
          <Field
            id="email"
            name="email"
            type="email"
            class="form-control"
            :class="{ 'is-invalid': errors['email'] }"
            placeholder="請輸入電子郵件"
            rules="email|required"
            v-model="form.user.email"
          ></Field>
          <ErrorMessage
            name="email"
            class="invalid-feedback"
          ></ErrorMessage>
        </div>

        <div class="mb-3">
          <label for="tel" class="form-label">收件人電話<span class="px-1" style="color: #f00;">*</span></label>
          <Field
            id="tel"
            name="電話"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['電話'] }"
            placeholder="請輸入電話"
            rules="required|min:8|max:10"
            v-model="form.user.tel"
          ></Field>
          <ErrorMessage
            name="電話"
            class="invalid-feedback"
          ></ErrorMessage>
        </div>

        <div class="mb-3">
          <label for="address" class="form-label">收件人地址<span class="px-1" style="color: #f00;">*</span></label>
          <Field
            id="address"
            name="地址"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['地址'] }"
            placeholder="請輸入地址"
            rules="required"
            v-model="form.user.address"
          ></Field>
          <ErrorMessage
            name="地址"
            class="invalid-feedback"
          ></ErrorMessage>
        </div>

        <div class="mb-3">
          <label for="payment" class="form-label">付款方式<span class="px-1" style="color: #f00;">*</span></label>
          <select id="payment" class="w-100">
            <option value="">信用卡</option>
            <option value="">ATM</option>
            <option value="">超商繳費</option>
            <option value="">ApplePay</option>
            <option value="">LinePay</option>
          </select>
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
      </Form>
    </div>
  </div>
  <div class="w-50">
    <div class="couponPart text-center">
      {{carts}}
      <div class="coupon">
        <div class="couponImage fs-3">
          <div class="couponTxt">
            夏日特賣
            <br>折扣碼:777
          </div>
        </div>
        <div class="couponNum w-50 d-flex justify-content-center">
          <input class="d-flex" type="text" placeholder="輸入折扣碼:777">
          <button class="d-flex">套用</button>
        </div>
      </div>
    </div>
    <table class="table">
      <tbody v-for="(item,id) in carts.carts" :key="id">
        <tr>
          <td>
            <img :src="item.product.imageUrl" :alt="item.product.title">
          </td>
          <td>{{item.product.title}}
            <br>NT ${{item.product.price}}/份
          </td>
          <td>X{{item.qty}}</td>
        </tr>
      </tbody>
    </table>
    <!-- <div class="payment text-center mb-5 fs-5 d-flex align-items-center justify-content-center">
      <div class="">
        <label for="pay">付款方式:</label>

      </div>
      <div class="pay text-center my-3 fs-5 ml-3">
        <button type="button" class="btn btn-danger" style="color:#fff">
          完成結帳
        </button>
      </div>
    </div> -->
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
      carts: [],
      loadingStatus: {
        loadingItem: ''
      },
      isLoading: false,
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
      this.$http
        .get(url)
        .then((response) => {
          this.carts = response.data.data
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    },
    createOrder () {
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/order`
      const order = this.form
      this.$http.post(url, { data: order }).then((response) => {
        this.$refs.form.resetForm() // vee validate 的方法 form reset
        console.log(response.data.orderId)
        this.getCart()
        // this.$router.push('/payment')
      }).catch((err) => {
        this.$httpMessageState(err.response, '送出訂單')
      })
    }
    // getOrder (id) {
    //   const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/order/${id}`
    //   this.$http
    //     .get(url)
    //     .then((response) => {
    //       this.order = response.data.
    //     })
    //     .catch((err) => {
    //       alert(err.response.data.message)
    //     })
    // }
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
