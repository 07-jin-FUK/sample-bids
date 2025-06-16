<template>
    <div>
  <aside class="sidebar" :class="{ collapsed: isCollapsed }" @mouseenter="hoverExpand" @mouseleave="hoverCollapse">
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
  </aside>
      <button class="toggle-btn" @click="toggleSidebar">↔︎</button>
</div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const isCollapsed = ref(false)
const router = useRouter()

const toggleSidebar = () => {
  isCollapsed.value = !isCollapsed.value
}

const hoverExpand = () => {
  if (isCollapsed.value) {
    document.querySelector('.sidebar').classList.add('hover-expand')
  }
}

const hoverCollapse = () => {
  document.querySelector('.sidebar').classList.remove('hover-expand')
}

const handleLogout = () => {
  // ログアウト処理（簡易）
  alert('ログアウトしました')
  router.push('/')
}
</script>

<style lang="scss" scoped>
.sidebar {
  background-color: #333;
  color: #fff;
  width: 220px;

  height: 100vh;
  position: fixed;
  transition: width 0.3s ease;
}

.sidebar.collapsed {
  width: 0px;
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
/* トグルボタン */
.toggle-btn {
  position: fixed;
  top: 56px;
  left: 220px; /* 通常時の位置 */
  background: #fff;
  color: #333;
  padding: 5px 10px;
  z-index: 1100;
  font-size: 1.5em;
  transition: left 0.3s ease;
}

.sidebar.collapsed + .toggle-btn {
  left: 0px; /* サイドバーが閉じたときの位置 */
}
</style>
