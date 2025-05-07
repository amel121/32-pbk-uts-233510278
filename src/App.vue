<script setup>
import { ref, computed } from 'vue'

const newActivity = ref('')
const activities = ref([])
const filterOption = ref('all') 

const addActivity = () => {
  if (newActivity.value.trim()) {
    activities.value.push({ text: newActivity.value.trim(), done: false })
    newActivity.value = ''
  }
}

const removeActivity = (index) => {
  activities.value.splice(index, 1)
}

const resetAllActivities = () => {
  activities.value = []
}

const filteredActivities = computed(() => {
  if (filterOption.value === 'done') {
    return activities.value.filter((a) => a.done)
  } else if (filterOption.value === 'not_done') {
    return activities.value.filter((a) => !a.done)
  }
  return activities.value
})
</script>

<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <form class="form-group" @submit.prevent="addActivity">
      <input v-model="newActivity" placeholder="Tambah kegiatan baru" />
      <button type="submit">Tambah</button>
      <button type="button" @click="resetAllActivities">Reset Semua</button>
    </form>

    <div class="filters">
      <label for="filter">Filter Kegiatan:</label>
      <select id="filter" v-model="filterOption">
        <option value="all">Semua pilihan Tampilkan</option>
        <option value="not_done">Tampilkan hanya yang belum selesai</option>
        <option value="done">Tampilkan hanya yang sudah selesai</option>
      </select>
    </div>

    <ul>
      <li v-for="(activity, index) in filteredActivities" :key="index">
        <input type="checkbox" v-model="activity.done" />
        <span :class="{ done: activity.done }">{{ activity.text }}</span>
        <button class="cancel-btn" @click="removeActivity(index)">Hapus</button>
      </li>
    </ul>
  </div>
</template>
