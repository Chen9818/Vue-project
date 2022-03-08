<template>
  <nav class="navbar navbar-expand-lg position-fixed w-100">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
        <img src="@/assets/logo.png" alt="logo" />
      </a>

      <div class="d-flex justify-content-end">
        <div class="d-md-block d-lg-none mx-4">
          <router-link class="p-4" to="/carts">
          <h1>
            <i class="bi bi-cart" style="color:#fff;font-size:2.5rem"></i>
          </h1>
          </router-link>
        </div>

        <button class="navbar-toggler navbar-dark d-md-block d-lg-none h-50 my-auto"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
      </div>

      <div class="collapse navbar-collapse " id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link fs-3 text-center" to="/">首頁</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link fs-3 text-center" to="/products">商品列表</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link fs-3 text-center" to="/about">關於我們</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link fs-3 text-center" to="/question">常見問答</router-link>
          </li>
        </ul>
      </div>
    </div>
      <div class="cart-icon d-none d-lg-block p-4">
        <router-link to="/carts">
          <h1>
            <i class="bi bi-cart" style="color:#fff;font-size:3.5rem"></i>
          </h1>
        </router-link>
      </div>
  </nav>
</template>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";
.navbar {
  background-color: $base-color;
  z-index: 2;
  .navbar-brand img {
    width: 4.5rem;
    height: 4.5rem;
  }
  .navbar-toggler {
    border-color: #fff;
    color: $text;
  }
  .nav-link {
    color: #efefef;
    transform: translateY(0px);
    transition: 0.3s;
    &:hover {
          transform: translateY(-5px);
        }
    &:after {
      content: "";
      position: absolute;
      left: 50%;
      right: 50%;
      bottom: -5px;
      height: 0;
      border-bottom: 1px solid #fff;
      transition: 0.3s;
    }
    &:hover:after {
      right: 20%;
      left: 20%;
    }
  }
}
</style>
