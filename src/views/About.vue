<template>
    <div class="about w-100" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title="MainTitle"></MainImage>
      <div class="history">
        <div class="title mb-3">
          <h1>会社の宗旨</h1>
        </div>
        <div class="txt fs-4 p-4">
          「好眠」秉持著為您提供最好的睡眠相關諮詢。
          我們的產品都經過專業研發團隊反覆測試及評估，為各種睡眠需求提供最佳的解決辦法。
          我們相信好的睡眠品質與枕頭品質有極大的關聯。
          我們將幫助您選對適合自己的枕頭，以改善睡眠品質。「好眠」祝您一夜好眠。
        </div>
      </div>
      <div class="location pt-3">
        <div class="title">
          <h1>店の情報</h1>
        </div>
        <div>
          <div class="d-flex locationTitle">台北信義店
            <span class="d-flex align-items-end d-none d-md-flex">
              (地址:台灣台北市信義區信義路五段7號)
            </span>
          </div>
          <div class="location1 w-100" >
            <GMapMap
            :center="center1"
            :zoom="17"
            map-type-id="terrain"
            class="map mx-auto"
            >
            <GMapCluster>
              <GMapMarker
                  :key="index"
                  v-for="(m, index) in markers"
                  :position="m.position"
                  :clickable="true"
                  :draggable="true"
                  @click="center=m.position"
              />
              </GMapCluster>
            </GMapMap>
          </div>
        </div>
        <div style="width:100%;">
          <div class="d-flex locationTitle">高雄夢時代店
            <span class="d-flex align-items-end d-none d-md-flex">
              (地址:806台灣高雄市前鎮區中華五路789號)
            </span>
          </div>
          <div class="location2 w-100">
            <GMapMap
            :center="center2"
            :zoom="17"
            map-type-id="terrain"
            class="map mx-auto"
            >
            <GMapCluster>
              <GMapMarker
                  :key="index"
                  v-for="(m, index) in markers"
                  :position="m.position"
                  :clickable="true"
                  :draggable="true"
                  @click="center=m.position"
              />
              </GMapCluster>
            </GMapMap>
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
  name: 'About',
  data () {
    return {
      MainTitle: '關於我們',
      isLoading: false,
      center1: { lat: 25.033953570933967, lng: 121.56509967426602 },
      center2: { lat: 22.59551597830121, lng: 120.3069220423625 },
      markers: [
        {
          id: 1,
          position: {
            lat: 25.033953570933967,
            lng: 121.56509967426602
          }
        },
        {
          id: 2,
          position: {
            lat: 22.59551597830121,
            lng: 120.3069220423625
          }
        }
      ]
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
.history,.location{
  .title{
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: $base-color;
  color: #fff;
  padding: .5rem;
  }
  .locationTitle{
    color:#000;
    font-size: 2.5rem;
    width: 70%;
    margin: auto;
    margin-top: 3rem;
    padding: 5px;
    border-bottom: 2px solid #000;
    span{
      font-size: 1rem;
    }
  }
  .txt{
  background: #fff;
  height: 50%;
  width: 50%;
  line-height:3rem;
  margin: auto;
  border-radius:.5rem
  }
}
.map{
  margin-top: 1rem;
  height:25rem;
  width:70%
}
@media (max-width: 500px){
  .map{
  height:15rem;
  }
}
@media (max-width: 830px){
  .history {
    .txt{
      width: 80%;
      line-height: 2rem;
    }
  }
}
</style>
