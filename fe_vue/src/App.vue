<template>
  <div class="app-container">
    <header class="navbar">
      <div class="logo">
        <img src="https://via.placeholder.com/40" alt="Logo" />
      </div>
      <nav class="nav-links">
        <router-link to="/" exact-active-class="active">Home</router-link>
        <router-link to="/predict" active-class="active">Predict</router-link>
        <router-link to="/history" active-class="active">History</router-link>
      </nav>
    </header>

    <main class="content">
      <router-view />
    </main>

    <footer class="footer">
      <p>&copy; {{ displayYear }} C303. Developed by Widi Arrohman at PENS.</p>
    </footer>
  </div>
</template>

<script setup>
import { computed } from 'vue';

const displayYear = computed(() => {
  const currentYear = new Date().getFullYear();
  return currentYear > 2026 ? `2026-${currentYear}` : '2026';
});
</script>

<style>
/* CSS Global agar sesuai mockup */
.app-container {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  max-width: 900px;
  margin: 0 auto;
  border: 1px solid #000;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.navbar {
  display: flex;
  align-items: center;
  padding: 10px 20px;
  border-bottom: 2px solid #000;
  gap: 20px;
}

.nav-links a {
  text-decoration: none;
  color: black;
  padding: 5px 15px;
  border: 1px solid transparent;
}

/* Style tombol biru saat aktif */
.nav-links a.active {
  background-color: #a0c4ff;
  border: 1px solid #000;
}

.content {
  flex: 1;
  padding: 20px;
}

.footer {
  border-top: 1px solid #000;
  padding: 10px;
  text-align: center;
  font-size: 0.8em;
}
</style>