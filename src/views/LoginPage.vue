<template>
  <div class="login">
    <h2>Login</h2>
    <form @submit.prevent="handleLogin">
      <input type="email" placeholder="Email" v-model="email" required />
      <input type="password" placeholder="Password" v-model="password" required />
      <button type="submit">Login</button>
      <p>
        Don't have an account?
        <router-link to="/signup">Sign Up</router-link>
      </p>
    </form>
  </div>
</template>


<script setup>
  import { ref } from 'vue'
  import { useRouter } from 'vue-router'
  import { useUserStore } from '../store/userStore'

  const email = ref('')
  const password = ref('')
  const router = useRouter()
  const userStore = useUserStore()

  const handleLogin = () => {
    const users = JSON.parse(localStorage.getItem('users')) || []

    const foundUser = users.find(
      user => user.email === email.value && user.password === password.value
    )

    if (!foundUser) {
      alert('Invalid credentials')
      return
    }

    localStorage.setItem('currentUser', JSON.stringify(foundUser))

    userStore.setUser(foundUser)

    router.push('/dashboard')
  }
</script>


<style scoped>
  .login {
    max-width: 400px;
    margin: 100px auto;
    padding: 2rem;
    border-radius: 10px;
    background: #ffffff;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
  }

  .login h2 {
    text-align: center;
    margin-bottom: 1.5rem;
    color: #333;
  }

  .login input {
    display: block;
    width: 100%;
    margin-bottom: 1rem;
    padding: 0.8rem;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-size: 1rem;
  }

  .login button {
    width: 100%;
    padding: 0.8rem;
    background-color: #4f46e5;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 1rem;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .login button:hover {
    background-color: #4338ca;
  }

  .login p {
    margin-top: 1rem;
    text-align: center;
    font-size: 0.9rem;
  }

  .login a {
    color: #4f46e5;
    text-decoration: none;
  }
</style>