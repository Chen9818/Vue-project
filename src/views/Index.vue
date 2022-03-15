<template>
  <div>
    <ToastMessage></ToastMessage>
    <router-view></router-view>
  </div>
</template>

<script>
import ToastMessage from '@/components/ToastMessage.vue'

export default {
  components: { ToastMessage }
}
</script>
