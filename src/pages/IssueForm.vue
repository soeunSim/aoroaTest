<script setup>
import { useRoute, useRouter } from 'vue-router'
import { ref, computed } from 'vue'
import { issues, users } from '../data/mockData'

const route = useRoute()
const router = useRouter()

const isNew = route.path === '/issues/new'

const current = ref(
  isNew
    ? {
        title: '',
        description: '',
        status: 'PENDING',
        user: null,
      }
    : (() => {
        const found = issues.find((issue) => {
          return issue.id === Number(route.params.id)
        })

        return {
          title: found.title,
          description: found.description,
          status: found.status,
          user: found.user || null,
        }
      })(),
)

const canChangeStatus = computed(() => {
  return current.value.user && !['COMPLETED', 'CANCELLED'].includes(current.value.status)
})

const canChangeUser = computed(() => {
  return !['COMPLETED', 'CANCELLED'].includes(current.value.status)
})

function save() {
  if (isNew) {
    const newIssue = {
      id: Date.now(),
      title: current.value.title,
      description: current.value.description,
      status: current.value.user ? 'IN_PROGRESS' : 'PENDING',
      user: current.value.user,
      createdAt: new Date().toISOString(),
      updatedAt: new Date().toISOString(),
    }

    issues.push(newIssue)
    console.log('새 이슈 저장:', newIssue)
  } else {
    console.log('이슈 수정:', current.value)
  }

  router.push('/issues')
}
</script>

<template>
  <div>
    <h2>{{ isNew ? '이슈 생성' : '이슈 수정' }}</h2>

    <label>제목</label>
    <input v-model="current.title" />

    <label>설명</label>
    <textarea v-model="current.description" />

    <label>상태</label>
    <select v-model="current.status" :disabled="!canChangeStatus">
      <option value="PENDING">PENDING</option>
      <option value="IN_PROGRESS">IN_PROGRESS</option>
      <option value="COMPLETED">COMPLETED</option>
      <option value="CANCELLED">CANCELLED</option>
    </select>

    <label>담당자</label>
    <select v-model="current.user" :disabled="!canChangeUser">
      <option :value="null">선택 안함</option>
      <option v-for="user in users" :key="user.id" :value="user">
        {{ user.name }}
      </option>
    </select>

    <button @click="save">저장</button>
    <button @click="router.push('/issues')">목록으로</button>
  </div>
</template>
