<template>
  <nav class="navbar navbar-expand-lg w-100">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">
      <img src="@/assets/logo.png" alt="logo" />
    </a>
    <button class="navbar-toggler navbar-dark" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
          <router-link class="nav-link" to="/">首頁</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/products">商品列表</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/admin/products">後台產品列</router-link>
        </li>
      </ul>
    </div>
    <div class="cart-icon">
      <router-link class="nav-link" to="/carts">
        <svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" fill="currentColor" class="bi bi-cart3" viewBox="0 0 16 16" style="color:#fff;">
          <path d="M0 1.5A.5.5 0 0 1 .5 1H2a.5.5 0 0 1 .485.379L2.89 3H14.5a.5.5 0 0 1 .49.598l-1 5a.5.5 0 0 1-.465.401l-9.397.472L4.415 11H13a.5.5 0 0 1 0 1H4a.5.5 0 0 1-.491-.408L2.01 3.607 1.61 2H.5a.5.5 0 0 1-.5-.5zM3.102 4l.84 4.479 9.144-.459L13.89 4H3.102zM5 12a2 2 0 1 0 0 4 2 2 0 0 0 0-4zm7 0a2 2 0 1 0 0 4 2 2 0 0 0 0-4zm-7 1a1 1 0 1 1 0 2 1 1 0 0 1 0-2zm7 0a1 1 0 1 1 0 2 1 1 0 0 1 0-2z"/>
        </svg>
        購物車
      </router-link>
    </div>
  </div>
</nav>
</template>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";
.navbar {
  position: fixed;
  background-color: $base-color;
  z-index: 2;
  .navbar-brand img {
    width: 80px;
    height: 80px;
  }
  .navbar-toggler {
  border-color: #fff;
  color: #fff;
  }
  .nav-link {
    color: #fff;
  }
}
</style>
