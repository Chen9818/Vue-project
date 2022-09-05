<template>
<Loading :active="isLoading"></Loading>
<div class="text-end mt-4">
  <button class="btn btn-primary" type="button" @click="openCouponModal(true)">
    新しいクーポンを増やす
  </button>
</div>
<table class="table table-hover mt-4">
  <thead>
    <tr>
      <th scope="col"></th>
      <th scope="col">商品</th>
      <th scope="col">割引</th>
      <th scope="col">クーポン締切</th>
      <th scope="col">起用</th>
      <th scope="col">編輯</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(item,id) in coupon" :key="id">
      <th scope="row"></th>
      <td>{{item.title}}</td>
      <td>{{item.percent}}%</td>
      <td>{{$filters.date(item.due_date)}}</td>
      <td>
        <div v-if="item.is_enabled === 1">起用</div>
        <div v-else>起用しない</div>
      </td>
      <td>
        <button type="button" class="btn btn-outline-primary"
        @click="openCouponModal(false, item)">編輯</button>
        <button type="button" class="btn btn-outline-danger"
        @click="openDelCouponModal(item)">取り消し</button>
      </td>
    </tr>
  </tbody>
</table>
<CouponModal :coupon="tempCoupon" :is-new="isNew" ref="couponModal" @update-coupon="updateCoupon"></CouponModal>
<DeleteProductModal :item="tempCoupon" ref="delModal" @del-item="delCoupon"></DeleteProductModal>
<PaginationView
class="d-flex justify-content-center"
:pages="pagination"
@emitPages="getCoupon"
></PaginationView>
</template>

<script>
import PaginationView from '@/components/PaginationView.vue'
import CouponModal from '@/components/admin/CouponModal.vue'
import DeleteProductModal from '@/components/admin/DeleteProductModal.vue'

export default {
  data () {
    return {
      coupon: [],
      tempCoupon: {
        title: '',
        is_enabled: 0,
        percent: 100,
        code: ''
      },
      isLoading: false,
      isNew: false,
      pagination: {}
    }
  },
  components: {
    PaginationView,
    CouponModal,
    DeleteProductModal
  },
  mounted () {
    const token = document.cookie.replace(
      /(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1'
    )
    this.$http.defaults.headers.common.Authorization = token
    if (token) this.getCoupon()
  },
  methods: {
    getCoupon (page = 1) {
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/coupons?page=${page}`
      this.$http.get(url)
        .then((response) => {
          const { coupons, pagination } = response.data
          this.coupon = coupons
          this.pagination = pagination
          console.log(this.coupon, this.pagination)
        }).catch((err) => {
          alert(err.response.data.message)
        })
    },
    openCouponModal (isNew, item) {
      this.isNew = isNew
      if (this.isNew) {
        this.tempCoupon = {
          due_date: new Date().getTime() / 1000
        }
      } else {
        this.tempCoupon = { ...item }
      }
      this.$refs.couponModal.openModal()
    },
    openDelCouponModal (item) {
      this.tempCoupon = { ...item }
      const delComponent = this.$refs.delModal
      delComponent.openModal()
    },
    updateCoupon (tempCoupon) {
      this.isLoading = true
      let url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/coupon`
      let httpMethods = 'post'
      let data = tempCoupon

      if (!this.isNew) {
        url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/coupon/${this.tempCoupon.id}`
        httpMethods = 'put'
        data = this.tempCoupon
      }

      this.$http[httpMethods](url, { data }).then((response) => {
        this.isLoading = false
        this.$httpMessageState(response, '新しいクーポン')
        this.getCoupon()
        this.$refs.couponModal.hideModal()
      }).catch((error) => {
        this.isLoading = false
        this.$httpMessageState(error.response, 'エーラー')
      })
    },
    delCoupon () {
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/coupon/${this.tempCoupon.id}`
      this.isLoading = true
      this.$http.delete(url).then((response) => {
        this.isLoading = false
        this.$httpMessageState(response, 'クーポンを取り消す')
        const delComponent = this.$refs.delModal
        delComponent.hideModal()
        this.getCoupon()
      }).catch((error) => {
        this.isLoading = false
        this.$httpMessageState(error.response, 'クーポンを取り消す')
      })
    }
  }
}
</script>
