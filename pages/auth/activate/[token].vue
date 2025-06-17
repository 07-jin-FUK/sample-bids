<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
        <div class="resistation-form">
      <h2 class="title-bottom">アカウント登録</h2>
      <p class="sub-title-bottom">下記情報を入力してください。</p>
      <form class="form" @submit.prevent="handleSubmit">
        <div class="form-content">
          <label class="label">会社名</label>
          <input type="text" v-model="name" required />
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
          <label class="label">ご担当者</label>
          <input type="text" v-model="contact" required />
        </div>
             <div class="form-content">
          <label class="label">役職</label>
          <input type="text" v-model="job" required />
        </div>
        <div class="form-content">
          <label class="label">部署名</label>
          <input type="tel" v-model="department" />
        </div>
        <div class="form-content">
          <label class="label">メールアドレス</label>
          <input type="email" v-model="email" required />
        </div>

        <div class="button-center">
          <button type="submit" class="button">ログイン</button>
        </div>
      </form>
      </div>
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

// URLからtokenを取得
const route = useRoute()
const token = route.params.token

// ダミーデータ（本来はAPIから取得）
const email = ref('dummy-user@example.com')

// onMountedでAPI風の処理を模倣（ここは仮）
onMounted(() => {
  console.log('本登録トークン:', token)
  // 本来はここでAPIから email を取得する流れです
  // 例: /api/activate?token=xxxxx
})

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

<style lang="scss" scoped>
.title-bottom {
  margin-bottom: 30px;
  text-align: center;
  font-weight: normal;
  letter-spacing: 0.6px;
  font-size: 1.25em;
}

.sub-title-bottom {
  margin-bottom: 25px;
  text-align: center;
  font-weight: normal;
  letter-spacing: 0.6px;
  font-size: 0.875em;
}

.page-wrap {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-image: 
  linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)),
  url('/images/img-auth-common-use.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.main {
display: flex;
justify-content: center;
margin:  150px auto;
}

.resistation-form{
      width: 380px;
  height: 700px;
  background-color: #fff;
  padding: 50px 60px;
}

.form-content {
  margin-bottom: 20px;
}



input {
  width: 92%;
  padding: 2.5px 15px;
  font-size: 1em;
  border: 1px solid #707070;
  border-radius: 5px;
}

.button-wrap {
  text-align: center;
}

.button-center{
    text-align: center;
    margin-top: 30px;
    margin-bottom: 10px;
button {
    text-align: center;
    font-size: 0.75em;
    color: #fff;
    background-color: #000;
    padding: 10px 20px;
    border-radius: 5px;
    letter-spacing: 0.36px;
    line-height: 1;
  }
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
