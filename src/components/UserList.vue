<template>
  <div>
    <input
      v-model="search"
      type="text"
      placeholder="Buscar por nombre"
      class="mb-4 p-2 border rounded w-full"
    />

    <div v-if="filteredUsers.length" class="grid md:grid-cols-2 gap-4">
      <div
        v-for="user in filteredUsers"
        :key="user.login.uuid"
        class="p-4 border rounded shadow hover:shadow-lg transition"
      >
        <img :src="user.picture.medium" class="rounded-full mb-2 w-20 h-20" />
        <h2 class="text-lg font-semibold">
          {{ user.name.first }} {{ user.name.last }}
        </h2>
        <p>{{ user.email }}</p>
        <p>{{ user.location.country }}</p>
      </div>
    </div>
    <div v-else>
      <p>No se encontraron usuarios.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'

const users = ref([])
const search = ref('')

onMounted(async () => {
  const res = await fetch('https://randomuser.me/api/?results=20')
  const data = await res.json()
  users.value = data.results
})

const filteredUsers = computed(() =>
  users.value.filter((user) =>
    `${user.name.first} ${user.name.last}`.toLowerCase().includes(search.value.toLowerCase())
  )
)
</script>
