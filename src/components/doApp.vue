<template>
  <div class="todo-app">
    <h1>Daftar kegiatan</h1>
    
    <!-- Form untuk menambahkan kegiatan baru -->
    <form @submit.prevent="addActivity">
      <input type="text" v-model="newActivity" placeholder="Tambahkan kegiatan baru">
      <button type="submit">Tambah</button>
    </form>

    <!-- Daftar kegiatan -->
    <ul class="activities-list">
      <li v-for="activity in filteredActivities" :key="activity.id">
        <input type="checkbox" v-model="activity.completed">
        <span :class="{ completed: activity.completed }">{{ activity.name }}</span>
        <button @click="cancelActivity(activity.id)">Hapus</button>
      </li>
    </ul>
    
    <!-- Filter kegiatan yang belum selesai -->
    <label>
      <input type="checkbox" v-model="showIncompleteOnly">
      Tampilkan yang belum selesai saja
    </label>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const activities = ref([
  { id: 1, name: 'Makan siang', completed: false },
  { id: 2, name: 'Belajar VueJS', completed: false }
]);

const newActivity = ref('');
const showIncompleteOnly = ref(false);

function addActivity() {
  if (newActivity.value.trim() !== '') {
    activities.value.push({ id: Date.now(), name: newActivity.value, completed: false });
    newActivity.value = '';
  }
}

function cancelActivity(id) {
  const index = activities.value.findIndex(activity => activity.id === id);
  if (index !== -1) {
    activities.value.splice(index, 1);
  }
}

function completeActivity(activity) {
  activity.completed = !activity.completed;
}

const filteredActivities = computed(() => {
  if (showIncompleteOnly.value) {
    return activities.value.filter(activity => !activity.completed);
  } else {
    return activities.value;
  }
});
</script>

<style scoped>
/* Gaya CSS untuk komponen TodoApp */
.todo-app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-family: Arial, sans-serif;
  background-color: #f8f8f8;
}

/* Gaya CSS untuk judul */
h1 {
  text-align: center;
  color: #333;
}

/* Gaya CSS untuk form */
form {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  border: 2px solid #ddd;
  border-radius: 4px;
  outline: none;
}

button[type="submit"] {
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  background-color: #66bb6a;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button[type="submit"]:hover {
  background-color: #4caf50;
}

/* Gaya CSS untuk daftar kegiatan */
.activities-list {
  list-style-type: none;
  padding: 0;
}

.activities-list li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.activities-list li:last-child {
  border-bottom: none;
}

.activities-list li input[type="checkbox"] {
  margin-right: 10px;
}

.activities-list li .activity-name {
  flex: 1;
  font-size: 16px;
  color: #333;
}

.activities-list li button {
  padding: 8px 12px;
  font-size: 14px;
  color: white;
  background-color: #ff6666;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.activities-list li button:hover {
  background-color: #cc0000;
}
</style>
