<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <h2>ようこそ 入札システムへ</h2>
      <h2 class="design">トップページのデザインがここに入ります</h2>

      <div class="button-group">
<NuxtLink to="/auth/login" class="action-button">ログイン</NuxtLink>
<NuxtLink to="/auth/register" class="action-button white">新規会員登録</NuxtLink>
      </div>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'
</script>

<style scoped>


.main {
  flex: 1;
  text-align: center;
  padding: 40px;
}

.design {
  margin-top: 60px;
}

.button-group {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 40px;
  flex-wrap: wrap;
}

.action-button {
  display: inline-block;
  padding: 12px 24px;
  background-color: #007bff;
  color: white;
  text-decoration: none;
  border-radius: 8px;
  font-weight: bold;
  min-width: 150px;
  text-align: center;
  transition: 0.3s;
}

.action-button:hover {
  background-color: #0056b3;
}

.action-button.white {
  background-color: white;
  color: #007bff;
  border: 2px solid #007bff;
}

.action-button.white:hover {
  background-color: #f0f8ff;
}
</style>
