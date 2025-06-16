<template>


  <div class="page-wrap">
    <main class="main">

      <h2 class="title">入札会一覧</h2>
    

      <table class="bids-table">
        <thead>
          <tr>
            <th></th>
            <th>主催</th>
            <th>カテゴリー</th>
            <th>タイプ</th>
            <th>入札会名称</th>
            <th>入札期間</th>
             <th>状態</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="bid in mockBids" :key="bid.id">
            <td>{{ bid.id }}</td>
            <td>{{ bid.host }}</td>
            <td>{{ bid.category }}</td>
            <td>{{ bid.type }}</td>
            <td>{{ bid.name }}</td>
            <td>{{ bid.date }}</td>
            <td>
              <div v-if="bid.status === '募集中'">
                <NuxtLink :to="`/customer/auction/${bid.id}`" class="join-button">参加する</NuxtLink>
              </div>
              <div v-else>
                {{ bid.status }}
              </div>
            </td>
          </tr>
        </tbody>
      </table>

    </main>
  </div>
</template>

<script setup>
import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

// 仮のモックデータ
const mockBids = [
  {
    id: 101,
    host:'日本--',
    category: 'T&M(先頭個品NO)',
    type: '複数品',
    name: '2504入札会',
    date: '2025-04-17 09:00:00～ 2025-04-30 17:00:00',
        status: '募集中'
  },
  {
    id: 102,
    title: '2025年7月度 OA機器入札会',
    date: '2025-07-05',
    deadline: '2025-07-03',
    status: '準備中'
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

.join-button {
  display: inline-block;
  color:  #007bff;
  border-radius: 4px;
  text-decoration: none;
}

.back-to-mypage {
  display: inline-block;
  margin-bottom: 20px;
  color: #007bff;
  text-decoration: none;
  font-weight: bold;
}
.back-to-mypage:hover {
  text-decoration: underline;
}

</style>
