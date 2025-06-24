<template>
  <div class="page-wrap">
    <Header />
    <Sidebar />
    <main class="main">

      <!-- 仮落札案件 -->
      <section class="section-block">
        <h2 class="title">仮落札一覧</h2>
        <table class="bids-table" v-if="temporaryWinningList.length">
          <thead>
            <tr>
              <th>案件ID</th>
              <th>タイトル</th>
              <th>締切日時</th>
              <th>現在の状況</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="bid in temporaryWinningList" :key="bid.id">
              <td>{{ bid.id }}</td>
              <td>{{ bid.title }}</td>
              <td>{{ bid.deadline }}</td>
              <td>
                <NuxtLink :to="`/customer/biddingprogress/${bid.id}`" class="link-button">詳細へ</NuxtLink>
              </td>
            </tr>
          </tbody>
        </table>
        <p v-else class="no-data">仮落札案件はありません。</p>
      </section>

      <!-- 入札期間中 -->
      <section class="section-block">
        <h2 class="title">入札期間中の参加中入札会</h2>
        <table class="bids-table" v-if="openBiddingList.length">
          <thead>
            <tr>
              <th>案件ID</th>
              <th>タイトル</th>
              <th>締切日時</th>
              <th>現在の状況</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="bid in openBiddingList" :key="bid.id">
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
      </section>

    </main>
    <Footer />
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const allBids = [
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
    status: '仮落札'
  },
  {
    id: 303,
    title: '8月度 試験用案件',
    deadline: '2025-08-01 09:00',
    status: '入札中'
  }
]

// フィルター
const temporaryWinningList = allBids.filter(bid => bid.status === '仮落札')
const openBiddingList = allBids.filter(bid => bid.status === '入札中')
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

.section-block {
  margin-bottom: 60px;
}

.title {
  margin-bottom: 20px;
  text-align: left;
  font-size: 1.2rem;
  font-weight: bold;
}

.bids-table {
  width: 100%;
  border-collapse: collapse;
  background: #fff;
}

.bids-table th,
.bids-table td {
  border: 0.3px solid #707070;
  text-align: left;
  vertical-align: middle;
  line-height: 1;
  padding: 10px 15px ;
    font-size: 0.875em;
}


.bids-table td {
color: #1F1F1F;
}

.bids-table th {
  background-color: #000000;
  color: #ffffff;
}

.bids-table tr:hover {
  background-color: #f0f8ff;
}

.no-data {
  text-align: center;
  font-size: 1rem;
  color: #888;
  margin-top: 20px;
}

.link-button {
  background: none;
  border: none;
  color: #007bff;
  text-decoration: underline;
  cursor: pointer;
}
</style>
