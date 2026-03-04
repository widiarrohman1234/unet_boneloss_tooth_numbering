<template>
  <div class="container">
    <h1>Prediction History</h1>

    <div v-if="history.length === 0">
      No prediction history yet.
    </div>

    <div v-for="item in history" :key="item.id" class="history-card">
      <img :src="item.result_image" />
      <div>
        <p><strong>Severity:</strong> {{ item.severity }}</p>
        <p><strong>Date:</strong> {{ item.created_at }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      history: [],
    };
  },
  async mounted() {
    try {
      const response = await axios.get(
        "http://localhost:5000/history"
      );
      this.history = response.data;
    } catch (error) {
      alert("Failed to load history");
    }
  },
};
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: auto;
  padding: 20px;
}

.history-card {
  display: flex;
  gap: 20px;
  border: 1px solid #ddd;
  padding: 15px;
  margin-bottom: 15px;
  border-radius: 8px;
}

.history-card img {
  width: 150px;
  border-radius: 6px;
}
</style>