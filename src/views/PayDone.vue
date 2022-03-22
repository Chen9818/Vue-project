<template>
  <div class="pay" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <div class="payDone">
      <div class="payImage">
        <div class="payText">完成付款</div>
      </div>
      <div class="d-flex justify-content-center">
        <button type="button" class="btn btn-base" style="color:#fff">回首頁</button>
      </div>
    </div>
    <FooterView></FooterView>
  </div>
</template>

<script>
import NavbarView from '@/components/NavbarView.vue'
import FooterView from '@/components/FooterView.vue'

export default {
  data () {
    return {
      title: '確認付款',
      isLoading: false
    }
  },
  components: {
    NavbarView,
    FooterView
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/base/all.scss';
.payDone{
  width: 100%;
  height: 100vh;
  padding: 10rem 0 10rem 0;
  .payImage{
    background: url('../assets/pic/main-page/summer.png');
    background-position: center center;
    background-repeat: no-repeat;
    background-size:50% 100%;
    text-align: center;
    height: 80vh;
    text-align: center;
    color:#fff;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-content: center;
    .payText{
      background: $base-color;
      text-align: center;
      margin: auto;
      width: 30%;
    }
  }
}

</style>
