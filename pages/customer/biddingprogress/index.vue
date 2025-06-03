<template>
  <div class="page-wrap">
    <Header />
    <main class="main">
                      <NuxtLink to="/customer" class="back-to-mypage">← マイページへ戻る</NuxtLink>

      <h2 class="title">進行中の入札</h2>

      <table class="bids-table" v-if="biddingList.length">
        <thead>
          <tr>
            <th>案件ID</th>
            <th>タイトル</th>
            <th>締切日時</th>
            <th>現在の状況</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="bid in biddingList" :key="bid.id">
            <td>{{ bid.id }}</td>
            <td>{{ bid.title }}</td>
            <td>{{ bid.deadline }}</td>
            <td>
              <NuxtLink :to="`/customer/biddingprogress/${bid.id}`" class="link-button">詳細へ</NuxtLink>
            </td>
          </tr>
        </tbody>
      </table>

      <p v-else class="no-data">現在、進行中の入札はありません。</p>

    </main>
    <Footer />
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const biddingList = [
  {
    id: 301,
    title: '6月度 建材入札',
    deadline: '2025-06-15 17:00',
    status: '入札中'
  },
  {
    id: 302,
    title: '7月度 電設資材入札',
    deadline: '2025-07-10 12:00',
    status: '見積作成中'
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
  max-width: 1000px;
  margin: auto;
}

.title {
  margin-bottom: 30px;
  text-align: center;
}

.bids-table {
  width: 100%;
  border-collapse: collapse;
  background: #fff;
}

.bids-table th,
.bids-table td {
  padding: 12px;
  border: 1px solid #ccc;
  text-align: center;
}

.bids-table th {
  background-color: #f5f5f5;
}

.bids-table tr:hover {
  background-color: #f0f8ff;
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
