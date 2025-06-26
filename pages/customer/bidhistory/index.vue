<template>
  <div class="page-wrap">
    <Header />
    <Sidebar />
    <main class="main">
      <section class="section-block">
        <h2 class="title">落札履歴</h2>
     <table v-if="openBiddingList.length" class="bids-table">
          <thead>
            <tr>
              <th>案件番号</th>
              <th>カテゴリー</th>
              <th>入札会名称</th>
              <th>締切日時</th>
              <th>落札数</th>
              <th>詳細</th>
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
    bid.items.filter(item => item.status === 'confirmed').length
  }} / {{ bid.items.length }} 件
</td>
              <td>
                <NuxtLink :to="`/customer/bidhistory/${bid.id}`" class="link-button">詳細</NuxtLink>
              </td>
            </tr>
          </tbody>
        </table>
      </section>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Sidebar  from '~/components/Sidebar.vue'  
import Footer from '~/components/Footer.vue'

const allBids = [
  {
    id: 301,
    projectId: '07334',
    category: "電気計測器",
    title: '6月度 建材入札',
    endDate: "2025年06月10日 17:00",
    items: [
  { id: 1, status: 'confirmed' },
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
  { id: 1, status: 'confirmed' },
  { id: 2, status: 'losing' },
  { id: 3, status: 'none' },
    { id: 4, status: 'confirmed' },
  { id: 5, status: 'losing' },
  { id: 6, status: 'none' },
    { id: 7, status: 'confirmed' },
  { id: 8, status: 'losing' },
  { id: 9, status: 'none' },
    ]
  }
]


const confirmedBiddingList = allBids.filter(bid =>
  bid.items.some(item => item.status === 'confirmed')
)

// テンプレートで使う名前はこれに統一
const openBiddingList = confirmedBiddingList


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
