<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <h2 class="title-bottom">パスワードをお忘れですか？</h2>
      <p class="description"><span>登録したメールアドレスを入力してください。</span><span>パスワード再設定用のリンクをお送りします。</span></p>
      <form class="form" @submit.prevent="handleForgetPassword">
        <div class="form-content">
          <label class="label" for="email">メールアドレス</label>
          <input type="email" id="email" v-model="email" required />
        </div>
        <div class="button-wrap">
          <button type="submit" class="button">送信する</button>
        </div>
      </form>
      <NuxtLink to="/auth/login" class="sub-link">ログイン画面へ戻る</NuxtLink>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const email = ref('')

const handleForgetPassword = () => {
  console.log('再設定メール送信処理（仮）', email.value)
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
  text-align: center;
}

.description {
  font-size: 0.95em;
  margin-bottom: 30px;
  color: #444;
  
  span{
    display: block;
  }
  
}

.form-content {
  margin-bottom: 20px;
  text-align: left;
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
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.button:hover {
  background-color: #0056b3;
}

.sub-link {
  display: block;
  margin-top: 20px;
  font-size: 0.9em;
  color: #007bff;
  text-decoration: underline;
}

.title-bottom{
    margin-bottom: 50px;
}

</style>
