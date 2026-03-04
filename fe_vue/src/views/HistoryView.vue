<template>
  <div class="app-container">
    <!-- <header class="navbar">
      <div class="logo">
        <img src="https://via.placeholder.com/40" alt="Logo" />
      </div>
      <nav class="nav-links">
        <a href="#">Home</a>
        <a href="#">Predict</a>
        <a href="#" class="active">History</a>
      </nav>
    </header> -->

    <main class="content">
      <h2>History Bone Loss Severity and Tooth Numbering</h2>

      <div v-for="(group, date) in historyData" :key="date" class="history-group">
        <div class="date-divider">
          <span class="date-text">{{ date }}</span>
          <div class="line"></div>
        </div>

        <div class="grid-container">
          <div v-for="item in group" :key="item.id" class="history-card">
            <div class="thumbnail-wrapper">
              <img :src="item.thumbnail" :alt="item.fileName" class="thumbnail" />
            </div>
            <div class="card-footer">
              <span class="file-name">{{ item.fileName }}</span>
              <button class="btn-show" @click="showDetail(item.id)">Show</button>
            </div>
          </div>
        </div>
      </div>

      <div class="pagination-container">
        <div class="pagination-box">
          <button class="page-btn">Back</button>
          <div class="page-numbers">
            <span class="page active">1</span>
            <span class="page">2</span>
            <span class="page">3</span>
            <span class="page">4</span>
            <span class="dots">...</span>
            <span class="page">8</span>
          </div>
          <button class="page-btn">Next</button>
        </div>
      </div>
    </main>

    <footer class="footer">
      <p>&copy; {{ displayYear }} C303. Developed by Widi Arrohman at PENS.</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

// Dummy Data yang dikelompokkan berdasarkan tanggal
const historyData = ref({
  '2026-03-04': [
    { id: 1, fileName: 'image_budi_20260403135123.png', thumbnail: 'https://placehold.co/600x400.png?text=X-Ray+Result' },
    { id: 2, fileName: 'image_budi_20260403135123.png', thumbnail: 'https://placehold.co/600x400.png?text=X-Ray+Result' },
    { id: 3, fileName: 'image_budi_20260403135123.png', thumbnail: 'https://placehold.co/600x400.png?text=X-Ray+Result' },
  ],
  '2026-03-03': [
    { id: 4, fileName: 'image_budi_20260403135123.png', thumbnail: 'https://placehold.co/600x400.png?text=X-Ray+Result' },
  ]
});

// Logic Tahun Dinamis
const displayYear = computed(() => {
  const currentYear = new Date().getFullYear();
  return currentYear > 2026 ? `2026-${currentYear}` : '2026';
});

const showDetail = (id) => {
  console.log("Showing detail for ID:", id);
  alert("Redirecting to detailed view for ID: " + id);
};
</script>

<style scoped>
.app-container {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  max-width: 900px;
  margin: 0 auto;
  border: 1px solid #000;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Header */
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
}
.nav-links a.active {
  background-color: #a0c4ff;
  border: 1px solid #000;
}

/* Content */
.content {
  flex: 1;
  padding: 20px;
}

/* Date Divider */
.date-divider {
  display: flex;
  align-items: center;
  margin: 20px 0;
}
.date-text {
  font-weight: bold;
  margin-right: 15px;
  font-family: monospace;
}
.line {
  flex: 1;
  height: 4px;
  background-color: #ccc;
  border-radius: 2px;
}

/* Grid Layout */
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
  margin-bottom: 40px;
}

/* Card Style */
.history-card {
  border: 2px solid #000;
  padding: 10px;
  background: white;
  box-shadow: 4px 4px 0px #000;
}
.thumbnail-wrapper {
  border: 1px solid #000;
  margin-bottom: 8px;
  overflow: hidden;
}
.thumbnail {
  width: 100%;
  height: auto;
  display: block;
}
.card-footer {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}
.file-name {
  font-size: 11px;
  align-self: flex-start;
  margin-bottom: 5px;
  word-break: break-all;
}
.btn-show {
  background: white;
  border: 2px solid #000;
  padding: 2px 20px;
  cursor: pointer;
  font-weight: bold;
  box-shadow: 2px 2px 0px #000;
}
.btn-show:hover {
  transform: translate(-1px, -1px);
  box-shadow: 3px 3px 0px #000;
}

/* Pagination */
.pagination-container {
  display: flex;
  justify-content: center;
  margin-top: 40px;
  margin-bottom: 20px;
}
.pagination-box {
  border: 2px solid #000;
  border-radius: 25px;
  padding: 10px 30px;
  display: flex;
  align-items: center;
  gap: 20px;
}
.page-btn {
  background: white;
  border: 2px solid #000;
  padding: 5px 15px;
  cursor: pointer;
  font-weight: bold;
}
.page-numbers {
  display: flex;
  gap: 10px;
  font-weight: bold;
}
.page.active {
  text-decoration: underline;
}

/* Footer */
.footer {
  border-top: 1px solid #000;
  padding: 10px;
  text-align: center;
  font-size: 0.8em;
}
</style>