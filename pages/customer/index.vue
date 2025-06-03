<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <div class="logout-wrap">
        <button class="logout-button" @click="handleLogout">ログアウト</button>
      </div>
      <div class="dashboard-header">
        <h2>ようこそ、{{ userName }} さん</h2>
        <NuxtLink to="/customer/profileedit" class="profile-edit">プロフィール編集</NuxtLink>
      </div>


      <p class="description">こちらはあなたのダッシュボードです。</p>

      <div class="dashboard-grid">
        <div class="dashboard-card">
          <h3>入札会一覧</h3>
          <NuxtLink to="/customer/auction" class="card-link">詳しく見る</NuxtLink>
        </div>
        <div class="dashboard-card">
          <h3>入札参加途中一覧</h3>
          <NuxtLink to="/customer/biddingprogress" class="card-link">詳しく見る</NuxtLink>
        </div>
        <div class="dashboard-card">
          <h3>落札案件一覧</h3>
          <NuxtLink to="/customer/winningbids" class="card-link">詳しく見る</NuxtLink>
        </div>
        <div class="dashboard-card">
          <h3>入札履歴</h3>
          <NuxtLink to="/customer/bidhistory" class="card-link">詳しく見る</NuxtLink>
        </div>
      </div>

    </main>
    <Footer />
  </div>
  
  <!-- モーダル表示部分を main の外、page-wrap 内に追加 -->
<div v-if="showLogoutModal" class="modal-overlay">
  <div class="modal-content">
    <p>ログアウトしました</p>
  </div>
</div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const userName = ref('フラッグ') // 本番ではログイン情報から取得予定
const showLogoutModal = ref(false)
const router = useRouter()

const handleLogout = () => {
  showLogoutModal.value = true
  setTimeout(() => {
    showLogoutModal.value = false
    router.push('/') // ← ログイン画面に遷移（擬似）
  }, 2000) // 2秒後に遷移
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
  max-width: 1200px;
  margin: auto;
}

.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

.profile-edit {
  font-size: 0.9em;
  padding: 10px 20px;
  border: 1px solid #007bff;
  border-radius: 6px;
  color: #007bff;
  text-decoration: none;
  font-weight: bold;
}

.profile-edit:hover {
  background-color: #f0f8ff;
}

.description {
  margin: 10px 0 30px;
  color: #555;
}


.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr); /* ← ここがポイント */
  justify-content: center;
  width: 100%;
  max-width: 1020px;
  gap: 20px;
}


.dashboard-card {
  width: 400px;
  aspect-ratio: 1 / 1; /* 正方形にする */
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.dashboard-card h3 {
  font-size: 1.1em;
  margin-bottom: 15px;
  color: #333;
}

.card-link {
  display: inline-block;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  font-weight: bold;
  border-radius: 5px;
  text-decoration: none;
  transition: background 0.3s;
}

.card-link:hover {
  background-color: #0056b3;
}

.logout-wrap {
  text-align: end;
  margin-bottom: 20px;
}

.logout-button {
  background-color: #dc3545;
  color: #fff;
  border: none;
  padding: 12px 24px;
  border-radius: 5px;
  cursor: pointer;
}

.logout-button:hover {
  background-color: #b02a37;
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
  font-size: 1.2em;
}

</style>
