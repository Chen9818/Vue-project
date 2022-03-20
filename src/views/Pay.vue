<template>
  <div class="pay" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title='title'></MainImage>
    <div class="">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">{{payMethod}}</th>
            <th scope="col">First</th>
            <th scope="col">Last</th>
            <th scope="col">Handle</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th scope="row">1</th>
            <td>Mark</td>
            <td>Otto</td>
            <td>@mdo</td>
          </tr>
          <tr>
            <th scope="row">2</th>
            <td>Jacob</td>
            <td>Thornton</td>
            <td>@fat</td>
          </tr>
          <tr>
            <th scope="row">3</th>
            <td colspan="2">Larry the Bird</td>
            <td>@twitter</td>
          </tr>
        </tbody>
      </table>
    </div>
    <FooterView></FooterView>
  </div>
</template>

<script>
import NavbarView from '@/components/NavbarView.vue'
import FooterView from '@/components/FooterView.vue'
import MainImage from '@/components/MainImage.vue'
import emitter from '../utility/emitter'

export default {
  data () {
    return {
      title: '確認付款',
      isLoading: false,
      payMethod: ''
    }
  },
  components: {
    NavbarView,
    FooterView,
    MainImage
  },
  methods: {
  },
  mounted () {
    emitter.on('change', (e) => {
      console.log(e)
      this.payMethod = e
    })
  }
}
</script>
