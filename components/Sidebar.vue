<template>
  <aside class="sidebar" :class="{ collapsed: isCollapsed }">
    <nav>
      <ul>
        <li><NuxtLink to="/customer/">入札会一覧</NuxtLink></li>
        <li><NuxtLink to="/customer/biddingprogress">入札参加途中一覧</NuxtLink></li>
        <li><NuxtLink to="/customer/winningbids">落札案件一覧</NuxtLink></li>
        <li><NuxtLink to="/customer/bidhistory">入札履歴</NuxtLink></li>
        <li><NuxtLink to="/customer/profileedit">プロフィール編集</NuxtLink></li>
        <li><a href="#" @click.prevent="handleLogout">ログアウト</a></li>
      </ul>
    </nav>
    <button class="toggle-btn" @click="toggleSidebar">☰</button>
  </aside>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const isCollapsed = ref(false)
const router = useRouter()

const toggleSidebar = () => {
  isCollapsed.value = !isCollapsed.value
}

const handleLogout = () => {
  // ログアウト処理（簡易）
  alert('ログアウトしました')
  router.push('/')
}
</script>

<style scoped>
.sidebar {
  background-color: #333;
  color: #fff;
  width: 220px;

  height: 100vh;
  position: fixed;
  transition: width 0.3s ease;
}

.sidebar.collapsed {
  width: 60px;
}

.sidebar ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.sidebar li {
  margin: 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1); /* 薄い白の区切り線 */
}

.sidebar a {
  display: block;
  padding: 16px;
  text-align: center;
  color: #fff;
  text-decoration: none;
  transition: background-color 0.2s ease, color 0.2s ease;
}

.sidebar a:hover {
  background-color: #444;
  color: #eee;
  border-radius: 0; /* 角丸不要なら0でもOK */
}
.toggle-btn {
  position: absolute;
  bottom: 20px;
  left: 20px;
  background: #333;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}
</style>
