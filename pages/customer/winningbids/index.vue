<template>
  <div class="page-wrap">
    <Header />
    <Sidebar />
    <main class="main">
      <section class="section-block">
        <h2 class="title">仮落札中入札会一覧</h2>
        <table class="bids-table" v-if="openBiddingList.length">
          <thead>
            <tr>
              <th>案件番号</th>
              <th>カテゴリー</th>
              <th>入札会名称</th>
              <th>締切日時</th>
              <th>仮落札数</th>
              <th>現在の状況</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="bid in openBiddingList" :key="bid.id">
              <td>{{ bid.projectId }}</td>
    <td>{{ bid.category }}</td>
              <td>{{ bid.title }}</td>
              <td>{{ bid.endDate }}</td>
             <td>
  {{
    bid.items.filter(item => item.status === 'winning').length
  }} / {{ bid.items.length }} 件
</td>
              <td>
                <NuxtLink :to="`/customer/winningbids/${bid.id}`" class="link-button">詳細へ</NuxtLink>
              </td>
            </tr>
          </tbody>
        </table>
        <p v-else class="no-data">現在、仮落札している入札会はありません。</p>
      </section>
    </main>
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const allBids = [
  {
    id: 301,
    projectId: '07334',
    category: "電気計測器",
    title: '6月度 建材入札',
    endDate: "2025年06月10日 17:00",
    items: [
      { id: 1, status: 'winning' },
      { id: 2, status: 'losing' },
      { id: 3, status: 'none' },
    ]
  },
  {
    id: 302,
    projectId: '07335',
    category: "電気計測器",
    title: '7月度 電設資材入札',
    endDate: "2025年07月30日 17:00",
    items: [
      { id: 1, status: 'none' },
      { id: 2, status: 'winning' },
    ]
  }
]


const openBiddingList = allBids.filter(bid =>
  bid.items.some(item => item.status === 'winning')
)


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
  margin: auto;
}

.section-block {
  margin-bottom: 60px;
}

.title {
  margin-bottom: 30px;
  text-align: left;
  font-size: 0.875em;
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
