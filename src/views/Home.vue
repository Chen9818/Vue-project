<template>
  <div class="home w-100">
    <NavbarView></NavbarView>
    <section class="main-image">
      <div
        class="main-txt d-flex align-items-center"
        data-aos="fade-zoom-in"
        data-aos-easing="ease-in-back"
        data-aos-delay="500"
        data-aos-duration="1000"
      >
        ~好眠~<br />いい睡眠を
      </div>
    </section>
    <section style="height:3vh;width:100%;background:#ccc"></section>
    <section class="main-banner">
      <div class="banner-txt">
        <h1>今の睡眠を満足していますか?</h1>
      </div>
      <div class="banner-img"></div>
    </section>
    <section class="info-part">
      <div
        class="info"
        data-aos="fade-right"
        data-aos-duration="1000"
        data-aos-once="true"
      >
        <div class="info-img">
          <img class="img-1" src="@/assets/pic/main-page/info-img.png" alt="使用枕頭目的" />
        </div>
        <div class="info-txt">
          <h3>
枕の目的</h3>
          <p>
            枕は頭の重さを支える睡眠時の首や肩への負担を軽減するものです。就寝中、首や肩には頭を支えるための負担が掛かっています。その重さは体重の10％弱で、きちんと支えないと寝ている間に首や肩が疲労することになります。 理想的な寝姿勢は頸椎が緩やかなS字カーブを描いた状態です。
          </p>
        </div>
      </div>
      <div
        class="info"
        data-aos="fade-left"
        data-aos-duration="1000"
        data-aos-once="true"
      >
        <div class="info-txt">
          <h3>枕の選び方？</h3>
          <p>
            枕の高さは、人それぞれの体型によって違います。立ったままの自然な姿勢を、横になったときも、そのままキープできるのが理想的です。
            <br />
            <span>仰向けの場合</span
            >：仰向け時に目線が真上を向くのではなく、真上から約5〜10度下がっている状態になっているかどうか。
            <br />
            <span>横向きの場合</span>：肩幅の分、より高さが必要。仰向けに比べて横向き寝用の両サイドは、通常2cm程度高くなります。
          </p>
        </div>
        <div class="info-img">
          <img class="img-2" src="@/assets/pic/main-page/info-img2.png" alt="如何挑選枕頭" />
        </div>
      </div>
    </section>
    <section class="product-view">
      <h2>商品一覽</h2>
    </section>
    <section class="product-type">
      <div class="items">
        <div class="item" @click="toType">
          <img src="@/assets/pic/機能枕/頂級機能枕.png" alt="機能枕" />
          <div class="txt">
            <h3>機能枕</h3>
          </div>
        </div>
        <div class="item" @click="toType">
          <img src="@/assets/pic/乳膠枕/頂級好眠乳膠枕.png" alt="乳膠枕" />
          <div class="txt">
            <h3>パイプ枕</h3>
          </div>
        </div>
        <div class="item" @click="toType">
          <img src="@/assets/pic/絲絨枕/頂級羊毛枕.png" alt="絲絨枕" />
          <div class="txt">
            <h3>わた枕</h3>
          </div>
        </div>
        <div class="item" @click="toType">
          <img src="@/assets/pic/兒童枕/卡納赫拉兒童枕.png" alt="兒童枕" />
          <div class="txt">
            <h3>児童枕</h3>
          </div>
        </div>
      </div>
    </section>
    <FooterView></FooterView>
  </div>
</template>
<script>
import FooterView from '@/components/FooterView.vue'
import NavbarView from '@/components/NavbarView.vue'

export default {
  name: 'Home',
  components: {
    FooterView,
    NavbarView
  },
  methods: {
    toType () {
      this.$router.push('/products')
    }
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";
@import "aos/src/sass/aos.scss";

.home {
  width: 100%;
}

.main-image {
  background: url(../assets/pic/main-page/main-img.png);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  width: 100%;
  height: 90vh;
  z-index: -1;
  position: relative;
  .main-txt {
    background: $base-color;
    border-radius: 50%;
    color: #fff;
    height: 30%;
    width: 60%;
    top: 100px;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    z-index: 2;
    font-size: 50px;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
  }
}
.main-banner {
  .banner-txt {
    background: url(../assets/pic/main-page/main-img2.png);
    background-size: cover;
    background-attachment: fixed;
    background-repeat: no-repeat;
    background-position: center center;
    z-index: -1;
    width: 100%;
    height: 60vh;
    position: relative;
    h1 {
      position: absolute;
      top: 50%;
      left: 50%;
      font-size: 60px;
      transform: translate(-50%, -50%);
      color: #fff;
    }
  }
}
.info-part {
  padding: 1.5rem;
  background: #ccc;
  z-index: -1;
  .info {
    margin: auto;
    justify-content: center;
    display: flex;
    width: 80%;
    font-size: 25px;
    background: #fff;
    border-radius: 10px;
    &:first-child {
      margin-bottom: 15vh;
    }
    .info-img {
      width: 400px;
      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
      .img-1{
        border-radius: 10px 0 0 10px;
      }
      .img-2{
        border-radius: 0 10px 10px 0;
      }
    }
    .info-txt {
      width: 900px;
      text-align: center;
      line-height: 50px;
      padding: 15px;
      p {
        margin-top: 10px;
        color: #aaa;
        text-align: start;
      }
      span {
        color: #f00;
      }
    }
  }
}
.product-view {
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
.product-type {
  width: 100%;
  background: #ccc;
  .items {
    width: 70%;
    padding: 1.5rem;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    margin: auto;
    .item {
      width: 40%;
      margin: 40px;
      position: relative;
      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        border-radius: 20px;
      }
      .txt {
        top: 0;
        left: 0;
        position: absolute;
        opacity: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5);
        transition: 0.6s;
        border-radius: 20px;
        display: flex;
        justify-content: center;
        align-items: center;
        h3 {
          text-align: center;
          font-size: 50px;
          color: #ccc;
        }
      }
      &:hover .txt {
        opacity: 1;
      }
    }
  }
}

@media (max-width: 900px) {
  .info-part {
    .info {
      display: block;
      .info-img {
        width: 100%;
        img {
          vertical-align: middle;
        }
        .img-1{
          border-radius: 10px 10px 0 0;
        }
        .img-2{
          border-radius: 0 0 10px 10px;
        }
      }
      .info-txt {
        width: 100%;
        text-align: center;
        padding: 15px;
        box-sizing: border-box;
      }
    }
  }
}
@media (max-width: 550px) {
  .info-part {
    .info {
      display: block;
      .info-img {
        width: 100%;
        img {
          vertical-align: middle;
        }
      }
      .info-txt {
        width: 100%;
        font-size: 10px;
        text-align: center;
        padding: 15px;
        box-sizing: border-box;
        p{
          line-height: 1.2rem;
        }
      }
    }
  }
}
@media (max-width: 1230px) {
  .product-type {
    .items {
      width: 100%;
      .item {
        width: 50%;
        height: 20rem;
      }
    }
  }
}
@media (max-width: 700px) {
  .product-type {
    .items {
      width: 100%;
      .item {
        width: 100%;
        height: 10rem;
      }
    }
  }
}
@media (max-width: 830px) {
  .main-header {
    .container {
      .main-nav {
        height: 0;
        width: 0;
        overflow: hidden;
      }
    }
    .icon {
      .bar-icon i {
        display: block;
      }
    }
  }
}
</style>
