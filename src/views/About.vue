<template>
    <div class="w-100" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title="MainTitle"></MainImage>
      <div class="history">
        <h3>創立宗旨</h3>
        <div>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptate, quidem dolores! Consectetur, eum! Necessitatibus unde nisi consequatur ut vel aperiam consectetur sit, soluta, libero, esse voluptatem exercitationem labore quia totam.
        </div>
      </div>
      <div class="location">
        <h3>實體店面</h3>
        <div>
          <h4>信義店</h4>
          <GMapMap
            :center="center"
            :zoom="17"
            map-type-id="terrain"
            class="a"
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
        <div style="width:1500px;height:80%">
          <h4>民權店</h4>
          <GMapMap
            :center="center1"
            :zoom="17"
            map-type-id="terrain"
            class="a"
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
      <!-- <div style="width:500px;height:80px">1111</div> -->
    </div>
    <FooterView></FooterView>
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
      center: { lat: 25.033953570933967, lng: 121.56509967426602 },
      center1: { lat: 25.133953570933967, lng: 121.66509967426602 },
      markers: [
        {
          id: 1,
          position: {
            lat: 25.133953570933967,
            lng: 121.66509967426602
          }
        },
        {
          id: 2,
          position: {
            lat: 25.033953570933967,
            lng: 121.56509967426602
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
// .a{
//   height:80px;
//   width:80px
// }
</style>
