<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import { issues } from '../data/mockData'

const router = useRouter()
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
  router.push(`/issues/${id}`)
}

function goToNewIssue() {
  router.push('/issues/new')
}
</script>

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
      신규 이슈 생성
    </button>
  </div>
</template>
