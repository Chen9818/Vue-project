<template>
  <div class="pay" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <div class="payDone">
      <div class="payImage">
        <div class="payText">完成付款</div>
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
  padding: 10rem 0 13rem 0;
  .payImage{
    background: url('../assets/pic/main-page/payDone.png');
    background-position: center center;
    background-repeat: no-repeat;
    background-size:80% 100%;
    height: 100vh;
    padding-bottom: 1rem;
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
      font-size: 3rem;
    }
  }
}

@media (max-width: 750px){
  .payDone .payImage{
    margin: 1rem;
    background-size:100% 100%;
    height: 50vh;
    .payText{
      font-size: 2rem;
    }
  }
}
</style>
