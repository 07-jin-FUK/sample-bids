<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <NuxtLink to="/customer" class="back-to-mypage">← マイページへ戻る</NuxtLink>
      <h2>落札案件一覧</h2>

      <table class="bid-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>案件名</th>
            <th>詳細</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="bid in winningBids" :key="bid.id">
            <td>{{ bid.id }}</td>
            <td>{{ bid.title }}</td>
            <td>
              <NuxtLink :to="`/customer/winningbids/${bid.id}`" class="link-button">
                詳細へ
              </NuxtLink>
            </td>
          </tr>
        </tbody>
      </table>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const winningBids = [
  {
    id: '101',
    title: '2025年度 電子機器調達'
  },
  {
    id: '102',
    title: '社内ネットワーク更新工事'
  },
  {
    id: '103',
    title: '備品購入（PC周辺機器）'
  }
]
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
  max-width: 900px;
  margin: auto;
}

.bid-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 30px;
}

.bid-table th,
.bid-table td {
  border: 1px solid #ccc;
  padding: 12px;
  text-align: left;
}

.bid-table th {
  background-color: #f0f0f0;
}

.link-button {
  color: #007bff;
  text-decoration: underline;
}
</style>
