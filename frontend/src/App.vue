<template>
  <main class="app-shell">
    <LoginForm />
  </main>
</template>

<script setup>
import LoginForm from './components/LoginForm.vue';
</script>

<style scoped>
.app-shell {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}
</style>
