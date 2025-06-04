<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <h2 class="title-bottom">仮登録</h2>
      <form class="form" @submit.prevent="handleRegister">
        <div class="form-content">
          <label class="label" for="email">メールアドレス</label>
          <input type="email" id="email" v-model="email" required />
        </div>
        <div class="form-content">
          <label class="label" for="password">パスワード</label>
          <input type="password" id="password" v-model="password" required />
        </div>
        <div class="button-wrap">
          <button type="submit" class="button">仮登録する</button>
        </div>
      </form>

      <NuxtLink to="/" class="sub-link">トップページへ戻る</NuxtLink>
      <NuxtLink to="/auth/login" class="sub-link">ログイン画面へ戻る</NuxtLink>
    </main>

    <!-- モーダル -->
    <div v-if="showModal" class="modal-overlay">
      <div class="modal-content">
        <p>
          仮登録処理をさせていただきました。<br />
          ご入力いただいたメールアドレス宛に<br />
          数日以内にご案内をお送りいたします。<br />
          今しばらくお待ちくださいませ。
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

const email = ref('')
const password = ref('')
const showModal = ref(false)
const router = useRouter()

const handleRegister = () => {
  console.log('仮登録処理（仮）', email.value, password.value)
  showModal.value = true

  setTimeout(() => {
    showModal.value = false
    router.push('/auth/RealRegister')
  }, 3000) // 3秒後に遷移
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

/* モーダルスタイル */
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
