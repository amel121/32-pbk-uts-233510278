<script setup>
import { ref, computed } from 'vue'

const newActivity = ref('')
const activities = ref([])
const showOnlyIncomplete = ref(false)

const addActivity = () => {
  if (newActivity.value.trim()) {
    activities.value.push({ text: newActivity.value.trim(), done: false })
    newActivity.value = ''
  }
}

const removeActivity = (index) => {
  activities.value.splice(index, 1)
}

const filteredActivities = computed(() => {
  return showOnlyIncomplete.value
    ? activities.value.filter((a) => !a.done)
    : activities.value
})
</script>

<template>
  <div class="container">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="addActivity">
      <input v-model="newActivity" placeholder="Tambah kegiatan baru" />
      <button type="submit">Tambah</button>
    </form>

    <div class="filters">
      <label>
        <input type="checkbox" v-model="showOnlyIncomplete" />
        Tampilkan hanya yang belum selesai
      </label>
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

<style>
body {
  margin: 0;
  padding: 0;
  min-height: 100vh;
  background-color: #7a607a; 
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
</style>

<style scoped>
.container {
  max-width: 700px;
  margin: 40px auto;
  padding: 30px;
  background: rgba(220, 151, 232, 0.8); 
  backdrop-filter: blur(8px);
  border-radius: 20px;
  box-shadow: 0 8px 20px rgba(64, 49, 49, 0.1);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  text-align: center;
  color: #9342a1; 
  margin-bottom: 25px;
  font-size: 30px;
  font-weight: bold;
}

form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 12px;
  border-radius: 10px;
  border: 1px solid #87659d; 
  font-size: 16px;
  transition: all 0.3s ease;
  background-color: #ffffff;
}

input[type="text"]:focus {
  outline: none;
  border-color: #a32bac; 
  box-shadow: 0 0 5px #da41bb;
}

button {
  padding: 12px 24px;
  background-color: #6a1b9a; 
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #ab47bc; 
}

.filters {
  margin-bottom: 20px;
  text-align: center;
  font-size: 15px;
  color: #6a1b9a; 
}

ul {
  padding: 0;
  list-style: none;
}

li {
  background: #c751e8; 
  padding: 15px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s;
}

li:hover {
  transform: translateY(-2px);
}

.done {
  text-decoration: line-through;
  color: #010101; 
  flex-grow: 1;
}

.cancel-btn {
  background: #c819b7; 
  color: rgb(235, 229, 229);
  padding: 6px 12px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-left: 60px;
}

.cancel-btn:hover {
  background: #3f1439; 
}
</style>
