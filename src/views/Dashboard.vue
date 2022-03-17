<template>
  <AdminNavbar @signOut='signOut'></AdminNavbar>
  <router-view></router-view>
</template>

<script>
import AdminNavbar from '@/components/AdminNavbar.vue'

export default {
  name: 'Dashboard',
  data () {
    return {
      checkSuccess: false
    }
  },
  components: {
    AdminNavbar
  },
  mounted () {
    this.checkLogin()
  },
  methods: {
    checkLogin () {
      const token = document.cookie.replace(
        /(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1'
      )
      if (token) {
        this.$http.defaults.headers.common.Authorization = token
        const url = `${process.env.VUE_APP_API}api/user/check`
        this.$http
          .post(url)
          .then(() => {
            this.checkSuccess = true
          })
          .catch((err) => {
            this.$httpMessageState(err.response, '錯誤訊息')
            this.$router.push('/login')
          })
      } else {
        this.$router.push('/login')
      }
    },
    signOut () {
      document.cookie = 'hexToken=;expires=;'
      alert('token 已清除')
      this.$router.push('/login')
    }
  }
}
</script>
