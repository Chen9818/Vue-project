<template>
    <div class="about w-100" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title="MainTitle"></MainImage>
      <div class="history pt-5">
        <div class="title mb-3">
          <h2>創立宗旨</h2>
        </div>
        <div class="txt">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptate, quidem dolores! Consectetur, eum! Necessitatibus unde nisi consequatur ut vel aperiam consectetur sit, soluta, libero, esse voluptatem exercitationem labore quia totam.
        </div>
      </div>
      <div class="location pt-3">
        <div class="title">
          <h2>實體店面</h2>
        </div>
        <div>
          <h4 class="d-flex">台北信義店
            <h5 class="d-flex align-items-end">
              (地址:台灣台北市信義區信義路五段7號)
            </h5>
          </h4>
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
          <h4 class="d-flex">高雄夢時代店
            <h5 class="d-flex align-items-end">
              (地址:806台灣高雄市前鎮區中華五路789號)
            </h5>
          </h4>
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
  height: 15vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: $base-color;
  color: #fff;
  padding: 10px;
  h2{
      font-size: 55px;
    }
  }
  h4{
    color:#fff;
    font-size: 2.5rem;
    width: 70%;
    margin: auto;
    margin-top: 3rem;
    padding: 5px;
    border-bottom: 2px solid $base-color;
  }
  .txt{
  background: #fff;
  height: 50%;
  width: 50%;
  margin: auto;
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
</style>
