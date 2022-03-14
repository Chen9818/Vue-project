<template>
    <div class="about w-100" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title="MainTitle"></MainImage>
    <div v-for="(item,id) in question" :key="id" class="mt-5">
      <p class="mx-auto" style="width:70%">
        <button class="btn btn-base w-100 fs-3" style="color:#fff;" type="button"
        data-bs-toggle="collapse" :data-bs-target="`#${item.A}`"
        aria-expanded="false" aria-controls="collapseExample">
          {{item.Q}}
        </button>
      </p>
      <div class="collapse mx-auto" :id="`${item.A}`" style="width:70%">
        <div class="card card-body fs-4">
          {{item.A}}
        </div>
      </div>
    </div>
    <FooterView class="mt-5"></FooterView>
    </div>
</template>

<script>
import FooterView from '@/components/FooterView.vue'
import MainImage from '@/components/MainImage.vue'
import NavbarView from '@/components/NavbarView.vue'

export default {
  name: 'Question',
  data () {
    return {
      MainTitle: '常見問答',
      isLoading: false,
      question: [
        { Q: '如何查詢包裹配送進度?', A: '您可以登入會員點選商品，再選擇配送進度即可查詢。' },
        { Q: '有實體店面嗎?', A: '本公司在台北、高雄兩地均有實體店面，歡迎您來店試用消費。' },
        { Q: '商品有七天鑑賞期嗎?', A: '本公司的商品均有七天鑑賞期，七天內可以退換貨。' },
        { Q: '如何退貨退款?', A: '您可以登入會員點選需退貨的商品，再選擇退貨退款即可進行退貨程序。' },
        { Q: '寄送地址填寫有錯，該如何解決?', A: '您可以在會員基本資料更新地址，或來店客服由專人為您服務。' },
        { Q: '如何取得優惠碼?', A: '您可以關注我們的FB粉絲團，優惠第一手消息都會在粉絲團。' }]
    }
  },
  components: {
    FooterView,
    NavbarView,
    MainImage
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";

</style>
