<template>
<nav class="navbar navbar-expand-lg">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">
        <img src="@/assets/logo.png" alt="logo"/>
    </a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
<div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav">
        <li class="nav-item">
            <router-link to="/">Home</router-link>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">產品列表</a>
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
</nav>
</template>

<style lang="scss" scoped>
@import '@/assets/base/all.scss';
.navbar {
    position: fixed;
    background-color: $base-color;
    height: 100px;
    width: 100%;
    z-index: 2;
    // color:#fff;
    .navbar-brand img{
        width: 100px;
        height: 80px;
    }
}
</style>

//     display: flex;
//     justify-content: space-between;
//     align-items: center;
//     width: 100%;
//   // position: relative;
//   .container {
//     display: flex;
//     align-items: center;
//     padding: 10px;
//     .logo {
//       width: 100px;
//       height: 80px;
//       img {
//         width: 100%;
//         height: 100%;
//         vertical-align: middle;
//       }
//     }
//     .main-nav {
//       display: flex;
//       a {
//         text-decoration: none;
//         color: #fff;
//         padding: 10px 0.5em;
//         position: relative;
//         transform: translateY(0px);
//         transition: 0.3s;
//         font-size: 30px;
//         &:hover {
//           transform: translateY(-5px);
//         }
//         &:after {
//           content: "";
//           position: absolute;
//           left: 50%;
//           right: 50%;
//           bottom: -5px;
//           height: 0;
//           border-bottom: 1px solid #fff;
//           transition: 0.3s;
//         }
//         &:hover:after {
//           right: 20%;
//           left: 20%;
//         }
//       }
//     }
//   }
//   .icon {
//     display: flex;
//     .cart-icon i,
//     .bar-icon i {
//       color: #fff;
//       font-size: 40px;
//       margin: 30px;
//     }
//     .bar-icon {
//       i {
//         display: none;
//       }
//       a {
//         text-decoration: none;
//       }
//     }
//   }
  // .collapse {
  //   position: absolute;
  //   top: 100%;
  //   left: 50%;
  // }
