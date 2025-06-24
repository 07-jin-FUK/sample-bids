<template>
  <div class="page-wrap">
    <Header />
    <Sidebar />
    <main class="main">
      <h2 class="title">チェック済み入札会</h2>

      <!-- 一覧テーブル -->
      <table class="bids-table" v-if="checkedList.length">
        <thead>
          <tr>
            <th>
              <span class="checkmark-indicator">✓</span>
            </th>
            <th>案件番号</th>
            <th>カテゴリー</th>
            <th>入札会名称</th>
            <th>入札開始</th>
            <th>入札終了</th>
            <th>詳細</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="bid in checkedList" :key="bid.id">
            <td>
              <label class="checkbox-label">
                <input type="checkbox" v-model="bid.checked" />
                <span class="checkmark">✓</span>
              </label>
            </td>
            <td>{{ bid.projectId }}</td>
            <td>{{ bid.category }}</td>
            <td>{{ bid.name }}</td>
            <td>{{ formatStartDate(bid.date) }}</td>
            <td>{{ formatEndDate(bid.date) }}</td>
            <td>
              <NuxtLink :to="`/customer/auction/${bid.id}`" class="join-button">
                詳細
              </NuxtLink>
            </td>
          </tr>
        </tbody>
      </table>

      <p v-else class="no-data">
        現在、チェック済みの入札会はありません。
      </p>

      <!-- “チェックを外す” ボタン -->
      <button
        v-if="isAnyUnchecked"
        class="remove-button"
        @click="submitUnchecked"
      >
        チェックを外す
      </button>

      <!-- 完了メッセージ -->
      <p v-if="showComplete" class="complete-msg">
        チェックを外しました
      </p>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import Header from '~/components/Header.vue'
import Sidebar from '~/components/Sidebar.vue'

const mockCheckedBids = ref([
  {
    id: 101,
    projectId: '07334',
    category: '電気計測器',
    name: '2504入札会',
    date: '2025年07月10日 09:00～ 2025年07月30日 17:00',
    checked: true,
  },
  {
    id: 102,
    projectId: '07332',
    category: 'OAその他',
    name: 'ICP-OESシステム 5100 Agilent',
    date: '2025年06月10日 09:00～ 2025年06月30日 17:00',
    checked: true,
  },
])

const checkedList = mockCheckedBids

const isAnyUnchecked = computed(() =>
  checkedList.value.some((bid) => !bid.checked)
)

const formatStartDate = (d) => d.split('～')[0].trim()
const formatEndDate = (d) => d.split('～')[1]?.trim() ?? ''

const showComplete = ref(false)

const submitUnchecked = async () => {
  const toRemove = checkedList.value.filter((b) => !b.checked)
  if (!toRemove.length) return

  try {
    await $fetch('/api/customer/uncheck', {
      method: 'POST',
      body: toRemove.map((b) => ({ projectId: b.projectId })),
    })
    checkedList.value = checkedList.value.filter((b) => b.checked)
    showComplete.value = true
    setTimeout(() => (showComplete.value = false), 3000)
  } catch (e) {
    console.error(e)
    alert('通信エラーが発生しました')
  }
}
</script>

<style scoped lang="scss">
.main {
  flex: 1;
  padding: 40px 60px;
  max-width: 1000px;
  margin: auto;
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
  th,
  td {
    border: 0.3px solid #707070;
    padding: 10px 15px;
    font-size: 0.875em;
  }
  th {
    background: #000;
    color: #fff;
  }
  tr:hover {
    background: #f0f8ff;
  }
}

.checkbox-label {
  display: flex;
  justify-content: center;
  cursor: pointer;
  input[type='checkbox'] {
    opacity: 0;
    width: 0;
    height: 0;
    position: absolute;
  }
  .checkmark {
    font-weight: bold;
 opacity: 0;
  transition: opacity 0.2s ease;
  }
  input[type='checkbox']:checked + .checkmark {
    opacity: 1;
  }
}

.remove-button {
  margin-top: 15px;
  padding: 10px 20px;
  background: #e60023;
  color: #fff;
  border: none;
  cursor: pointer;
}

.join-button {
  color: #007bff;
  text-decoration: underline;
}

.no-data {
  text-align: center;
  margin-top: 40px;
  color: #888;
}

.complete-msg {
  margin-top: 10px;
  color: #28a745;
  font-weight: bold;
}
</style>
