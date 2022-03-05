<template>
  <footer class="d-flex justify-content-around align-items-center">
    <h3 class="">僅用於個人學習使用，非商業用途。</h3>
    <button type="button" class="btn btn-outline-base mx-5" style="font-size:1.3rem">
      <router-link class="nav-link" to="/admin/products" style="color:#fff">後台管理</router-link>
    </button>
  </footer>
</template>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";
footer{
  height: 10vh;
  width: 100%;
  background-color: $base-color;
  color: #fff;
}
</style>
