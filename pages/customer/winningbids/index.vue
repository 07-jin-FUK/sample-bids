<template>
  <div class="page-wrap">
    <Header />
      <Sidebar />
    <main class="main">
      <h2>落札案件一覧</h2>

      <table class="bids-table">
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

<style scoped lang="scss">
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

.bids-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 30px;
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

.link-button {
  color: #007bff;
  text-decoration: underline;
}
</style>
