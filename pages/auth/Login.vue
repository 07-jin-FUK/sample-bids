<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <h2>ログイン</h2>
      <form class="form" @submit.prevent="handleLogin">
        <div class="form-content">
          <label class="label" for="email">メールアドレス</label>
          <input type="email" id="email" v-model="email" required />
        </div>
        <div class="form-content">
          <label class="label" for="password">パスワード</label>
          <input type="password" id="password" v-model="password" required />
        </div>
        <div class="button-wrap">
          <button type="submit" class="button">ログイン</button>
        </div>
      </form>
      <NuxtLink to="/auth/ForgetPassword" class="sub-link">パスワードをお忘れの方はこちら</NuxtLink>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'
import { useRouter } from 'vue-router' // 追加

const router = useRouter() // 追加
const email = ref('')
const password = ref('')

const handleLogin = () => {
  console.log('ログイン処理（仮）', email.value, password.value)
  router.push('/customer') 
  }
</script>

<style scoped>
.page-wrap {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main {
  flex: 1;
  padding: 40px;
  max-width: 400px;
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
  background-color: #222;
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
  text-align: center;
  font-size: 0.9em;
  color: #007bff;
  text-decoration: underline;
}
</style>
