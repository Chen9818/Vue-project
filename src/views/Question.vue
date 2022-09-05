<template>
    <div class="about w-100" style="background:#ccc">
    <Loading :active="isLoading"></Loading>
    <NavbarView></NavbarView>
    <MainImage :title="MainTitle"></MainImage>
    <div v-for="(item,id) in question" :key="id" class="mt-5">
      <p class="mx-auto" style="width:70%">
        <button class="btn btn-base w-100 fs-3" style="color:#fff;" type="button"
        data-bs-toggle="collapse" :data-bs-target="`#${item.A}`"
        aria-expanded="false" aria-controls="collapseExample">
          {{item.Q}}
        </button>
      </p>
      <div class="collapse mx-auto" :id="`${item.A}`" style="width:70%">
        <div class="card card-body fs-4">
          {{item.A}}
        </div>
      </div>
    </div>
    <FooterView class="mt-5"></FooterView>
    </div>
</template>

<script>
import FooterView from '@/components/FooterView.vue'
import MainImage from '@/components/MainImage.vue'
import NavbarView from '@/components/NavbarView.vue'

export default {
  name: 'Question',
  data () {
    return {
      MainTitle: 'よくあるご質問',
      isLoading: false,
      question: [
        { Q: '出荷状況をお知らせください。', A: '申し訳ございませんが、出荷後の出荷完了メールにてご案内させていただきます。' },
        { Q: '領収書の発行をお願いします。', A: 'マイページの購入履歴詳細画面から発行いただけます。詳しくはご利用ガイドをご覧ください。' },
        { Q: '保証期間はどのくらい？どういう保障がある？', A: '無償の保証期間は１年間です。保証をお使いになる場合、保証書が必要となります。それ以降は有償の修理対応となります。' },
        { Q: '代引き払いは可能でしょうか。', A: '申し訳ございませんが、代引きでのお支払いは受け付けておりません。' },
        { Q: '会員登録の住所を間違えました。どうしたら良いでしょうか。', A: 'ご登録のご住所変更につきましてはお手数になりますが、一度ログインをいただいてからお客様情報のご変更をいただくようになります。' },
        { Q: '海外への発送はできますか。', A: '日本国内からご注文をいただき、海外へ発送するご対応については現在お受けする事が叶いません。' }]
    }
  },
  components: {
    FooterView,
    NavbarView,
    MainImage
  }
}
</script>
