<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <h2 class="title-bottom">本会員登録</h2>
      <form class="form" @submit.prevent="handleSubmit">
        <div class="form-content">
          <label class="label">名称</label>
          <input type="text" v-model="name" required />
        </div>
        <div class="form-content">
          <label class="label">カナ</label>
          <input type="text" v-model="kana" required />
        </div>
        <div class="form-content">
          <label class="label">郵便番号</label>
          <input type="text" v-model="zipcode" required />
        </div>
        <div class="form-content">
          <label class="label">住所</label>
          <input type="text" v-model="address" required />
        </div>
        <div class="form-content">
          <label class="label">電話番号</label>
          <input type="tel" v-model="phone" required />
        </div>
        <div class="form-content">
          <label class="label">FAX番号</label>
          <input type="tel" v-model="fax" />
        </div>
        <div class="form-content">
          <label class="label">請求書は必要ですか？</label>
          <div>
            <label><input type="radio" value="yes" v-model="needsInvoice" /> 必要</label>
            <label><input type="radio" value="no" v-model="needsInvoice" /> 不要</label>
          </div>
        </div>

        <template v-if="needsInvoice === 'yes'">
          <div class="form-content">
            <label class="label">請求書郵便番号</label>
            <input type="text" v-model="invoiceZipcode" />
          </div>
          <div class="form-content">
            <label class="label">請求書住所</label>
            <input type="text" v-model="invoiceAddress" />
          </div>
          <div class="form-content">
            <label class="label">請求書部署</label>
            <input type="text" v-model="invoiceDepartment" />
          </div>
          <div class="form-content">
            <label class="label">請求書電話番号</label>
            <input type="tel" v-model="invoicePhone" />
          </div>
          <div class="form-content">
            <label class="label">請求書FAX番号</label>
            <input type="tel" v-model="invoiceFax" />
          </div>
          <div class="form-content">
            <label class="label">請求書担当者</label>
            <input type="text" v-model="invoicePerson" />
          </div>
          <div class="form-content">
            <label class="label">請求書敬称</label>
            <input type="text" v-model="invoiceTitle" />
          </div>
        </template>

        <div class="button-wrap">
          <button type="submit" class="button">本登録を送信する</button>
        </div>
      </form>
      <NuxtLink to="/" class="sub-link">トップページへ戻る</NuxtLink>
    </main>

    <div v-if="showModal" class="modal-overlay">
      <div class="modal-content">
        <p>
          ありがとうございます。<br />
          本登録審査に入らせていただきます。<br />
          登録が終了いたしますと、ログインが可能になります。
        </p>
      </div>
    </div>

    <Footer />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const router = useRouter()

const name = ref('')
const kana = ref('')
const zipcode = ref('')
const address = ref('')
const phone = ref('')
const fax = ref('')
const needsInvoice = ref('no')
const invoiceZipcode = ref('')
const invoiceAddress = ref('')
const invoiceDepartment = ref('')
const invoicePhone = ref('')
const invoiceFax = ref('')
const invoicePerson = ref('')
const invoiceTitle = ref('')
const showModal = ref(false)

const handleSubmit = () => {
  console.log('本登録情報:', {
    name: name.value,
    kana: kana.value,
    zipcode: zipcode.value,
    address: address.value,
    phone: phone.value,
    fax: fax.value,
    needsInvoice: needsInvoice.value,
    invoiceInfo: needsInvoice.value === 'yes' ? {
      zipcode: invoiceZipcode.value,
      address: invoiceAddress.value,
      department: invoiceDepartment.value,
      phone: invoicePhone.value,
      fax: invoiceFax.value,
      person: invoicePerson.value,
      title: invoiceTitle.value,
    } : null
  })
  showModal.value = true
  setTimeout(() => {
    showModal.value = false
    router.push('/auth/login')
  }, 3000)
}
</script>

<style scoped>
.title-bottom {
  margin-bottom: 50px;
}

.page-wrap {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main {
  flex: 1;
  padding: 40px;
  max-width: 500px;
  margin: auto;
}

.form-content {
  margin-bottom: 20px;
}

.label {
  font-weight: bold;
  display: block;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 10px 15px;
  font-size: 1em;
  border: 1px solid #aaa;
  border-radius: 4px;
}

.button-wrap {
  text-align: center;
}

.button {
  padding: 12px 24px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.button:hover {
  opacity: 0.8;
}

.sub-link {
  display: block;
  margin-top: 20px;
  font-size: 0.9em;
  color: #007bff;
  text-align: center;
  text-decoration: underline;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.modal-content {
  background: white;
  padding: 30px 40px;
  border-radius: 8px;
  text-align: center;
  font-weight: bold;
  font-size: 1em;
  line-height: 1.6;
}
</style>
