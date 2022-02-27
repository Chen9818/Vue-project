
<template>
  <div id="delProductModal" ref="delModal" class="modal fade" tabindex="-1" aria-labelledby="delProductModalLabel"
    aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content border-0">
        <div class="modal-header bg-danger text-white">
          <h5 id="delProductModalLabel" class="modal-title">
            <span>刪除產品</span>
          </h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          是否刪除
          <strong class="text-danger">{{ item.title }}</strong> 商品(刪除後將無法恢復)。
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">
            取消
          </button>
          <button type="button" class="btn btn-danger" @click="delProduct">
            確認刪除
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Modal from 'bootstrap/js/dist/modal'
let delProductModal = ''

export default {
  props: ['item', 'currentPage'],
  data () {
    return {
      modal: null
    }
  },
  mounted () {
    delProductModal = new Modal(this.$refs.delModal, {
      keyboard: false,
      backdrop: 'static'
    })
  },
  methods: {
    delProduct () {
      this.$http.delete(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/product/${this.item.id}`)
        .then((response) => {
          this.hideModal()
          this.$emit('update', this.currentPage)
        }).catch((error) => {
          alert(error.data.message)
        })
    },
    openModal () {
      delProductModal.show()
    },
    hideModal () {
      delProductModal.hide()
    }
  }
}
</script>
