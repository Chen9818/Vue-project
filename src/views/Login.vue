<template class="w-100">
  <div class="loginPage">
    <form class="justify-content-center" @submit.prevent="signIn">
      <div class="login">
        <div class="fs-1 mb-3 font-weight-normal text-center">注文管理ログイン</div>
        <div class="mb-2">
          <label for="inputEmail" class="sr-only fs-4">アカウント</label>
          <input
            type="email"
            id="inputEmail"
            class="form-control"
            placeholder="アカウント"
            v-model="user.username"
            required
            autofocus
          />
        </div>
        <div class="mb-2">
          <label for="inputPassword" class="sr-only fs-4">パスワード</label>
          <input
            type="password"
            id="inputPassword"
            class="form-control"
            v-model="user.password"
            placeholder="パスワード"
            required
          />
        </div>
        <div class="text-end mt-4 d-flex justify-content-around">
          <button class="btn btn-lg btn-base btn-block" style="color:#fff" type="submit">
            ログイン
          </button>
          <button class="btn btn-lg btn-base btn-block" style="color:#fff" type="submit" @click="mainPage()">
            商品ページへ
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      user: {}
    }
  },
  methods: {
    signIn () {
      const url = `${process.env.VUE_APP_API}admin/signin`
      this.$http
        .post(url, this.user)
        .then((response) => {
          const { token, expired } = response.data
          document.cookie = `hexToken=${token};expires=${new Date(expired)};`
          this.$router.push('/admin/products')
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    },
    mainPage () {
      this.$router.push('/')
    }
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/base/all.scss";
.loginPage{
  background: url(../assets/pic/main-page/main-img.png);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  form{
    padding: 1rem;
    .login{
      width: 50%;
      margin: auto;
      padding: 1rem;
      border:1px solid $base-color;
      backdrop-filter: blur(.5rem);
      .fs-1 ,.fs-4{
        color:#fff;
        text-shadow: 5px 5px 5px $base-color;
      }
    }
  }
}

@media (max-width: 550px) {
  .loginPage{
    form{
      .login {
        width: 100%;
      }
    }
  }
}
</style>
