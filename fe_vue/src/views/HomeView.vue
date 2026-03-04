<template>
  <div class="container">
    <h1>Dentex AI - Bone Loss Prediction</h1>

    <div class="card">
      <input type="file" @change="handleFileChange" accept="image/*" />

      <div v-if="preview" class="preview">
        <h3>Preview</h3>
        <img :src="preview" />
      </div>

      <button @click="predict" :disabled="loading || !file">
        {{ loading ? "Predicting..." : "Predict" }}
      </button>
    </div>

    <div v-if="resultImage" class="result">
      <h2>Prediction Result</h2>
      <img :src="resultImage" />
      <p><strong>Severity:</strong> {{ severity }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      file: null,
      preview: null,
      resultImage: null,
      severity: null,
      loading: false,
    };
  },
  methods: {
    handleFileChange(event) {
      this.file = event.target.files[0];
      this.preview = URL.createObjectURL(this.file);
    },
    async predict() {
      this.loading = true;

      const formData = new FormData();
      formData.append("image", this.file);

      try {
        const response = await axios.post(
          "http://localhost:5000/predict",
          formData
        );

        this.resultImage = response.data.result_image;
        this.severity = response.data.severity;
      } catch (error) {
        alert("Prediction failed");
      }

      this.loading = false;
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: auto;
  padding: 20px;
  text-align: center;
}

.card {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 10px;
}

.preview img,
.result img {
  max-width: 100%;
  margin-top: 15px;
}

button {
  margin-top: 15px;
  padding: 10px 20px;
  cursor: pointer;
}
</style>