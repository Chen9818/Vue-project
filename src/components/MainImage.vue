<template>
  <div class="main-img w-100">
    <h3>{{title}}</h3>
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  },
  props: ['title']
}
</script>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";

.main-img{
background: url(../assets/pic/main-page/main-img.png);
background-size: cover;
background-repeat: no-repeat;
background-position: center center;
background-attachment: fixed;
width: 100%;
height: 415px;
z-index: 2;
padding-top: 205px;
h3{
    font-size: 100px;
    color: #fff;
    text-shadow: 5px 5px 5px $base-color;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}

@media (max-width: 500px) {
  .main-img h3 {
    font-size: 5rem;
  }
}
</style>
