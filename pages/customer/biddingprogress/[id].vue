// File: BiddingProgress/[id].vue
<template>
  <div class="page-wrap">
    <Header />
       <Sidebar />
    <main class="main">
      <h2 class="title">入札詳細（ID: {{ bidId }}）</h2>
      <template v-if="bid">
        <h3>{{ bid.title }}</h3>
        <p>締切日時: {{ bid.deadline }}</p>
        <table class="bid-table">
          <thead>
            <tr>
              <th>ID</th>
              <th>アイテム名</th>
              <th>説明</th>
              <th>入札金額（円）</th>
              <th>備考</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in bid.items" :key="item.id">
              <td>{{ item.id }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.description }}</td>
              <td>{{ item.bidPrice }}</td>
              <td>{{ item.memo }}</td>
            </tr>
          </tbody>
        </table>
      </template>
      <p v-else>該当する入札情報が見つかりません。</p>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { ref, computed } from 'vue'
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const route = useRoute()
const bidId = route.params.id

const bids = [
  {
    id: '301',
    title: '6月度 建材入札',
    deadline: '2025-06-15 17:00',
    items: [
      { id: 1, name: '業務用プリンター', description: '高速プリンター', bidPrice: 85000, memo: '即納希望' },
      { id: 2, name: 'プロジェクター', description: '会議室用', bidPrice: 58000, memo: '' },
      { id: 3, name: 'IP電話機', description: 'クラウド対応電話', bidPrice: 42000, memo: '在庫確認済' }
    ]
  },
  {
    id: '302',
    title: '7月度 電設資材入札',
    deadline: '2025-07-10 12:00',
    items: [
      { id: 1, name: '電線ケーブル', description: '20m巻', bidPrice: 12000, memo: '即納可' },
      { id: 2, name: '分電盤', description: '家庭用', bidPrice: 34000, memo: '保証付き' }
    ]
  }
]

const bid = computed(() => bids.find(b => b.id === bidId))
</script>

<style scoped lang="scss">
.page-wrap {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main {
  flex: 1;
  padding: 40px;
  max-width: 1000px;
  margin: auto;
}

.title {
  margin-bottom: 20px;
  text-align: center;
}

.bid-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  background: #fff;
}

.bid-table th,
.bid-table td {
  padding: 10px;
  border: 1px solid #ccc;
  text-align: left;
}

.bid-table th {
  background-color: #f5f5f5;
}
</style>
