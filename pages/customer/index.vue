<template>
  <div class="page-wrap">
    <Header />
    <div class="dashboard-layout">
      <Sidebar />
      <main class="main-content">
        <AuctionIndex />
      </main>
    </div>
    <Footer />
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'
import Sidebar from '~/components/Sidebar.vue'
import AuctionIndex from '~/pages/customer/auction/index.vue' // 入札会一覧を直接読み込み

import { ref } from 'vue'
const userName = ref('フラッグ') // 本番はログイン情報から取得
</script>

<style scoped>
.page-wrap {
  display: flex;
  flex-direction: column;
}

.dashboard-layout {
  display: flex;
  flex: 1;
}

.main-content {
  flex: 1;
  padding: 40px;
  margin-left: 200px;
  h2{
    width: 100%;
    text-align: end;
  }
}

.description {
  margin: 10px 0 30px;
  color: #555;
}
</style>
