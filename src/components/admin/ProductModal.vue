<template>
  <div id="productModal" ref="modal" class="modal fade" tabindex="-1" aria-labelledby="productModalLabel"
    aria-hidden="true">
    <div class="modal-dialog modal-xl">
      <div class="modal-content border-0">
        <div class="modal-header bg-dark text-white">
          <h5 id="productModalLabel" class="modal-title">
            <span v-if="isNew">商品増やす</span>
            <span v-else>商品編集</span>
          </h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="row mb-3">
            <div class="col-sm-4">
              <div class="form-group mb-3">
                <label for="imageUrl" class="form-label">商品写真</label>
                    <div class="mb-3">
                      <label for="image" class="form-label">商品写真URL</label>
                      <input
                        type="text"
                        class="form-control"
                        id="image"
                        v-model="editProduct.imageUrl"
                        placeholder="商品写真URL"
                      />
                    </div>
                    <div class="mb-3">
                      <label for="customFile" class="form-label"
                        >或は写真アップロード
                        <i
                          class="fas fa-spinner fa-spin"
                          v-if="status.fileUploading"
                        ></i>
                      </label>
                      <input
                        type="file"
                        id="customFile"
                        class="form-control"
                        ref="fileInput"
                        @change="uploadFile"
                      />
                    </div>
                <img class="img-fluid" :src="editProduct.imageUrl">
              </div>
              <div v-if="Array.isArray(editProduct.imagesUrl)">
                <div class="mb-1" v-for="(image, key) in editProduct.imagesUrl" :key="key">
                  <div class="form-group">
                    <label for="imageUrl" class="form-label">写真URL</label>
                    <input v-model="editProduct.imagesUrl[key]" type="text" class="form-control"
                      placeholder="写真URL">
                  </div>
                  <img class="img-fluid" :src="image">
                </div>
                <div
                  v-if="!editProduct.imagesUrl.length || editProduct.imagesUrl[editProduct.imagesUrl.length - 1]">
                  <button class="btn btn-outline-primary btn-sm d-block w-100"
                    @click="editProduct.imagesUrl.push('')">
                    写真を増やす
                  </button>
                </div>
                <div v-else>
                  <button class="btn btn-outline-danger btn-sm d-block w-100" @click="editProduct.imagesUrl.pop()">
                    写真を取り消す
                  </button>
                </div>
              </div>
              <div v-else>
                <button class="btn btn-outline-primary btn-sm d-block w-100"
                  @click="createImages">
                  写真を増やす
                </button>
              </div>
            </div>
            <div class="col-sm-8">
              <div class="form-group mb-3">
                <label for="title" class="form-label">タイトル</label>
                <input id="title" v-model="editProduct.title" type="text" class="form-control" placeholder="タイトル">
              </div>

              <div class="row mb-3">
                <div class="form-group col-md-6">
                  <label for="category" class="form-label">商品種類</label>
                  <input id="category" v-model="editProduct.category" type="text" class="form-control"
                    placeholder="商品種類">
                </div>
                <div class="form-group col-md-6">
                  <label for="price" class="form-label">数量詞</label>
                  <input id="unit" v-model="editProduct.unit" type="text" class="form-control" placeholder="数量詞">
                </div>
              </div>

              <div class="row mb-3">
                <div class="form-group col-md-6">
                  <label for="origin_price" class="form-label">元の金額</label>
                  <input id="origin_price" v-model.number="editProduct.origin_price" type="number" min="0" class="form-control" placeholder="元の金額">
                </div>
                <div class="form-group col-md-6">
                  <label for="price" class="form-label">今の金額</label>
                  <input id="price" v-model.number="editProduct.price" type="number" min="0" class="form-control"
                    placeholder="今の金額">
                </div>
              </div>
              <hr>

              <div class="form-group mb-3">
                <label for="description" class="form-label">商品について</label>
                <textarea id="description" v-model="editProduct.description" type="text" class="form-control"
                  placeholder="商品について">
                </textarea>
              </div>
              <div class="form-group mb-3">
                <label for="content" class="form-label">說明內容</label>
                <textarea id="description" v-model="editProduct.content" type="text" class="form-control"
                  placeholder="說明內容">
                </textarea>
              </div>
              <div class="form-group mb-3">
                <div class="form-check">
                  <input id="is_enabled" v-model="editProduct.is_enabled" class="form-check-input" type="checkbox"
                    :true-value="1" :false-value="0">
                  <label class="form-check-label" for="is_enabled">起用</label>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">
            キャンセル
          </button>
          <button type="button" class="btn btn-primary" @click="updateProduct">
            確認
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Modal from 'bootstrap/js/dist/modal'
let productModal = ''

export default {
  props: ['product', 'isNew'],
  data () {
    return {
      modal: null,
      editProduct: {},
      status: {}
    }
  },
  mounted () {
    productModal = new Modal(this.$refs.modal, {
      keyboard: false,
      backdrop: 'static'
    })
  },
  watch: {
    product () {
      this.editProduct = this.product
    }
  },
  methods: {
    updateProduct () {
      // 新增商品
      let url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/product`
      let httpMethod = 'post'
      // 編輯商品
      if (!this.isNew) {
        url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/product/${this.product.id}`
        httpMethod = 'put'
      }
      this.$http[httpMethod](url, { data: this.editProduct })
        .then((response) => {
          alert(response.data.message)
          this.hideModal()
          this.$emit('update')
        }).catch((error) => {
          alert(error.response.data.message)
        })
    },
    createImages () {
      this.editProduct.imagesUrl = []
      this.editProduct.imagesUrl.push('')
    },
    uploadFile () {
      const uploadedFile = this.$refs.fileInput.files[0]
      console.log(this.$refs.fileInput.files[0]) // 選取第一個已選取資料
      const formData = new FormData()
      formData.append('file-to-upload', uploadedFile)
      const url = `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/admin/upload`
      this.status.fileUploading = true
      this.$http.post(url, formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      }).then((response) => {
        this.status.fileUploading = false
        if (response.data.success) {
          this.editProduct.imageUrl = response.data.imageUrl
          this.$refs.fileInput.value = ''
        } else {
          this.$refs.fileInput.value = ''
        }
      }).catch((error) => {
        this.status.fileUploading = false
        alert(error.response.data.message)
      })
    },
    openModal () {
      productModal.show()
    },
    hideModal () {
      productModal.hide()
    }
  }
}
</script>
