<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <h2 class="title-bottom">新規会員登録</h2>
      <form class="form" @submit.prevent="handleRegister">
        <div class="form-content">
          <label class="label" for="name">氏名</label>
          <input type="text" id="name" v-model="name" required />
        </div>
                <div class="form-content">
          <label class="label" for="name">企業名</label>
          <input type="text" id="name" v-model="name" required />
        </div>
        <div class="form-content">
          <label class="label" for="email">メールアドレス</label>
          <input type="email" id="email" v-model="email" required />
        </div>
        <div class="form-content">
          <label class="label" for="password">パスワード</label>
          <input type="password" id="password" v-model="password" required />
        </div>

                <div class="form-content">
          <label class="label" for="name">その他商談の上期間システムの企業情報項目を追加していく</label>
          <input type="text" id="name" v-model="name" required />
        </div>
                <div class="button-wrap">
          <button type="submit" class="button">登録する</button>
        </div>
      </form>
      <NuxtLink to="/" class="sub-link">トップページへ戻る</NuxtLink>
<NuxtLink to="/auth/login" class="sub-link">ログイン画面へ戻る</NuxtLink>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const name = ref('')
const email = ref('')
const password = ref('')

const handleRegister = () => {
  console.log('登録処理（仮）', name.value, email.value, password.value)
}
</script>

<style scoped>
.title-bottom{
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
  font-size: 0.9em;
  color: #007bff;
  text-align: center;
  text-decoration: underline;
}

</style>
