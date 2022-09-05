
<template>
  <div id="delProductModal" ref="delModal" class="modal fade" tabindex="-1" aria-labelledby="delProductModalLabel"
    aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content border-0">
        <div class="modal-header bg-danger text-white">
          <h5 id="delProductModalLabel" class="modal-title">
            <span>商品を取り消す</span>
          </h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          取り消しますか？
          <div v-if="item.title">
            <strong class="text-danger">{{ item.title }}</strong> 商品(回復できない)。
          </div>
          <div v-else>
            商品番号:<strong class="text-danger">{{ item.id }}</strong> オーダー(回復できない)。
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">
            キャンセル
          </button>
          <button type="button" class="btn btn-danger" @click="$emit('del-item')">
            確認
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
  props: ['item'],
  data () {
    return {
      modal: null
    }
  },
  emits: ['del-item'],
  mounted () {
    delProductModal = new Modal(this.$refs.delModal, {
      keyboard: false,
      backdrop: 'static'
    })
  },
  methods: {
    openModal () {
      delProductModal.show()
    },
    hideModal () {
      delProductModal.hide()
    }
  }
}
</script>
