// File: customer/bidhistory/[id].vue
<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
      <NuxtLink to="/customer/bidhistory" class="back-to-list">← 履歴一覧へ戻る</NuxtLink>
      <h2 class="title">入札履歴詳細（ID: {{ bidId }}）</h2>

      <template v-if="bid">
        <h3>{{ bid.title }}</h3>
        <p>入札日: {{ bid.date }}</p>

        <table class="detail-table">
          <thead>
            <tr>
              <th>ID</th>
              <th>アイテム名</th>
              <th>入札金額（円）</th>
              <th>当落</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in bid.items" :key="item.id">
              <td>{{ item.id }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.bidPrice }}</td>
              <td :class="{
                'won': item.result === '落札',
                'lost': item.result === '落選'
              }">{{ item.result }}</td>
            </tr>
          </tbody>
        </table>
      </template>

      <p v-else>該当する履歴情報が見つかりません。</p>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { computed } from 'vue'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const route = useRoute()
const bidId = route.params.id

const bidHistory = [
  {
    id: '201',
    title: '5月度 配管資材入札',
    date: '2025-05-10',
    items: [
      { id: 1, name: 'パイプA', bidPrice: 12000, result: '落札' },
      { id: 2, name: '継手B', bidPrice: 8000, result: '落札' }
    ]
  },
  {
    id: '202',
    title: '4月度 電気資材入札',
    date: '2025-04-05',
    items: [
      { id: 1, name: '照明C', bidPrice: 15000, result: '落札' },
      { id: 2, name: 'ケーブルD', bidPrice: 7000, result: '落選' }
    ]
  },
  {
    id: '203',
    title: '3月度 文具入札',
    date: '2025-03-01',
    items: [
      { id: 1, name: 'ボールペン', bidPrice: 500, result: '落選' },
      { id: 2, name: 'ノート', bidPrice: 300, result: '落選' }
    ]
  }
]

const bid = computed(() => bidHistory.find(b => b.id === bidId))
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
  max-width: 800px;
  margin: auto;
}

.title {
  text-align: center;
  margin-bottom: 20px;
}

.detail-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  background: #fff;
}

.detail-table th,
.detail-table td {
  padding: 10px;
  border: 1px solid #ccc;
  text-align: center;
}

.detail-table th {
  background-color: #f5f5f5;
}

.won {
  color: red;
  font-weight: bold;
}

.lost {
  color: black;
  font-weight: bold;
}

.back-to-list {
  display: block;
  margin-bottom: 20px;
  color: #007bff;
  text-decoration: underline;
}
</style>
