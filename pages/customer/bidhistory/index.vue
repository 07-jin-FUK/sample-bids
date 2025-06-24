// File: BiddingProgress/index.vue
<template>
  <div class="page-wrap">
    <Header />
      <Sidebar />
    <main class="main">
      <h2 class="title">過去の入札履歴</h2>

      <table class="history-table" v-if="bidHistory.length">
        <thead>
          <tr>
            <th>案件ID</th>
            <th>案件名</th>
            <th>入札日</th>
            <th>結果</th>
            <th>詳細</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="bid in bidHistory" :key="bid.id">
            <td>{{ bid.id }}</td>
            <td>{{ bid.title }}</td>
            <td>{{ bid.date }}</td>
            <td :class="{
              'won': bid.result === '全落札',
              'partial': bid.result === '一部落札',
              'lost': bid.result === '落選'
            }">{{ bid.result }}</td>
            <td><NuxtLink :to="`/customer/bidhistory/${bid.id}`" class="link-button">詳細</NuxtLink></td>
          </tr>
        </tbody>
      </table>

      <p v-else class="no-data">過去の入札履歴はありません。</p>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

// 仮のダミーデータ（将来的にAPIで取得）
const bidHistory = [
  {
    id: 201,
    title: '5月度 配管資材入札',
    date: '2025-05-10',
    result: '全落札'
  },
  {
    id: 202,
    title: '4月度 電気資材入札',
    date: '2025-04-05',
    result: '一部落札'
  },
  {
    id: 203,
    title: '3月度 文具入札',
    date: '2025-03-01',
    result: '落選'
  }
]
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
  margin-bottom: 30px;
  text-align: center;
}

.history-table {
  width: 100%;
  border-collapse: collapse;
  background: #fff;
}

.history-table th,
.history-table td {
  border: 0.3px solid #707070;
  text-align: left;
  vertical-align: middle;
  line-height: 1;
  padding: 10px 15px ;
    font-size: 0.875em;
}

.history-table td {
color: #1F1F1F;
}

.history-table th {
  background-color: #000000;
  color: #ffffff;
}

.history-table tr:hover {
  background-color: #f0f8ff;
}

.won {
  color: red;
  font-weight: bold;
}

.partial {
  color: orange;
  font-weight: bold;
}

.lost {
  color: black;
  font-weight: bold;
}

.no-data {
  text-align: center;
  font-size: 1rem;
  color: #888;
  margin-top: 50px;
}

.link-button {
  background: none;
  border: none;
  color: #007bff;
  text-decoration: underline;
  cursor: pointer;
}
</style>
