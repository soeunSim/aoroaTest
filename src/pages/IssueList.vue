<template>
  <div>
    <h1>이슈 목록</h1>

    <label for="statusFilter">상태 필터:</label>
    <select id="statusFilter" v-model="selectedStatus">
      <option value="">전체</option>
      <option value="PENDING">PENDING</option>
      <option value="IN_PROGRESS">IN_PROGRESS</option>
      <option value="COMPLETED">COMPLETED</option>
      <option value="CANCELLED">CANCELLED</option>
    </select>

    <ul>
      <li
        v-for="issue in filtered"
        :key="issue.id"
        @click="
          () => {
            goToDetail(issue.id)
          }
        "
      >
        {{ issue.title }} - {{ issue.status }} - {{ issue.user?.name || '미정' }}
      </li>
    </ul>

    <button
      @click="
        () => {
          goToNewIssue()
        }
      "
    >
      새 이슈 만들기
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { issues } from '../data/mockData'

const selectedStatus = ref('')

const filtered = computed(() => {
  if (selectedStatus.value) {
    return issues.filter((issue) => {
      return issue.status === selectedStatus.value
    })
  } else {
    return issues
  }
})

function goToDetail(id) {
  console.log(`이슈 상세 페이지 이동 (id: ${id})`)
}

function goToNewIssue() {
  console.log('새 이슈 생성 페이지로 이동')
}
</script>
