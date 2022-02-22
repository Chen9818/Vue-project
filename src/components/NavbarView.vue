<template>
  <nav class="navbar navbar-expand-lg">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
        <img src="@/assets/logo.png" alt="logo" />
      </a>
      <button
        class="navbar-toggler navbar-dark"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNavAltMarkup"
        aria-controls="navbarNavAltMarkup"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
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
            <a class="nav-link" href="#">關於我們</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">常見問答</a>
          </li>
        </ul>
      </div>
    </div>
    <div class="cart-icon">
      <!--***-->
      <i class="bi bi-cart"></i>
    </div>
  </nav>
</template>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";
.navbar {
  position: fixed;
  background-color: $base-color;
  height: 100px;
  width: 100%;
  z-index: 2;
  .navbar-brand img {
    width: 100px;
    height: 80px;
  }
  .navbar-toggler {
  border-color: #fff;
  color: #fff;
  text-decoration: none;
  }
  .nav-link {
    color: #fff;
  }
}
</style>
