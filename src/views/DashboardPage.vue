<template>
  <div class="dashboard" v-if="user">
    <h2>Welcome, {{ user.email }}</h2>
    <p>This is your dashboard.</p>
    <button @click="logout">Logout</button>
  </div>
  <div v-else>
    <p>You are not logged in.</p>
    <router-link to="/">Login</router-link>
  </div>
</template>

<script setup>
  import { onMounted, computed } from 'vue'
  import { useUserStore } from '../store/userStore'
  import { useRouter } from 'vue-router'

  const router = useRouter()
  const userStore = useUserStore()

  const user = computed(() => userStore.user)

  onMounted(() => {
    const storedUser = JSON.parse(localStorage.getItem('currentUser'))

    if (storedUser) {
      userStore.setUser(storedUser)
    } else {
      router.push('/')
    }
  })

  const logout = () => {
    localStorage.removeItem('currentUser')
    userStore.logout()
    router.push('/')
  }
</script>


<style scoped>
  .dashboard {
    max-width: 600px;
    margin: 100px auto;
    padding: 2rem;
    background: #f9fafb;
    border-radius: 10px;
    text-align: center;
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.08);
  }

  .dashboard h2 {
    font-size: 1.8rem;
    margin-bottom: 1rem;
    color: #111827;
  }

  .dashboard p {
    font-size: 1.1rem;
    color: #374151;
    margin-bottom: 2rem;
  }

  .dashboard button {
    padding: 0.8rem 1.5rem;
    font-size: 1rem;
    background-color: #ef4444;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .dashboard button:hover {
    background-color: #dc2626;
  }
</style>