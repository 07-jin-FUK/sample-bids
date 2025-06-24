<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <div class="new-comer-text">
        <h2 class="title-bottom">パスワード再設定</h2>
        <p class="first-text">
          以下のメールアドレスのパスワードを再設定してください。
          <br />
          <strong>{{ email }}</strong>
        </p>

        <form class="form" @submit.prevent="handleReset">
          <div class="form-content">
            <label class="label" for="newPassword">新しいパスワード</label>
            <input type="password" id="newPassword" v-model="newPassword" required />
          </div>
          <div class="form-content">
            <label class="label" for="newPassword">確認用</label>
        <input type="password" v-model="confirmPassword" required />
          </div>

          <div class="button-center">
            <button type="submit" class="button">再設定する</button>
          </div>
        </form>
      </div>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const route = useRoute()
const router = useRouter()
const token = route.params.token

// ✅ ダミーデータとして仮に取得済みとする（実際はAPIで取得）
const email = ref('dummy@example.com')
const newPassword = ref('')

// パスワード再設定処理（ダミー）
const handleReset = async () => {
  console.log('再設定トークン:', token)
  console.log('新パスワード:', newPassword.value)
  alert('パスワードを再設定しました（ダミー）')
  router.push('/auth/login')
}
</script>

<style scoped lang="scss">

.main {
  display: flex;
  justify-content: center;
  margin: 150px auto;
}

.new-comer-text {
  width: 430px;
  background-color: #fff;
  padding: 50px 60px;
}

.title-bottom {
  margin-bottom: 30px;
  text-align: center;
  font-weight: normal;
  font-size: 1.25em;
}

.first-text {
  font-size: 0.875em;
  text-align: center;
  margin-bottom: 20px;
  line-height: 1.6;
}

form {
  margin-top: 20px;
}

.page-wrap {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background-image: linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)),
    url('/images/img-auth-common-use.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.form-content {
  margin-bottom: 20px;
}

label {
  margin-left: 22px;
}

input {
  width: 81%;
  padding: 2.5px 15px;
  font-size: 1em;
  margin-left: 22px;
  border: 1px solid #707070;
  border-radius: 5px;
}

.button-center {
  text-align: center;

  button {
    font-size: 0.75em;
    color: #fff;
    background-color: #000;
    padding: 10px 20px;
    border-radius: 5px;
    letter-spacing: 0.36px;
    line-height: 1;
  }

  button:hover {
    opacity: 0.8;
  }
}
</style>

