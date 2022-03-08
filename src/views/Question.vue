<template>
    <div class="about w-100" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title="MainTitle"></MainImage>

    <div v-for="(item,id) in question" :key="id">
      <p>
        <button class="btn btn-base" style="color:#fff" type="button"
        data-bs-toggle="collapse" data-bs-target="#a"
        aria-expanded="false" aria-controls="collapseExample">
          {{item.Q}}
        </button>
      </p>
      <div class="collapse" id="a">
        <div class="card card-body">
          {{item.A}}
        </div>
      </div>
    </div>
    <FooterView></FooterView>
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
      question: [{ Q: '你好', A: 'hi' }, { Q: '你好1', A: 'hi2' }]
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
