<template>
  <div class="app-container">
    <main class="content">
      <h2>Predict Bone Loss Severity and Tooth Numbering</h2>

      <div class="section">
        <button class="btn" @click="triggerUpload">Upload Image</button>
        <input 
          type="file" 
          ref="fileInput" 
          @change="handleFileUpload" 
          accept="image/*" 
          style="display: none" 
        />
        
        <div class="image-preview" v-if="rawImage">
          <img :src="rawImage" alt="Uploaded X-Ray" />
        </div>
      </div>

      <div class="section" v-if="rawImage">
        <button 
          class="btn" 
          @click="startPrediction" 
          :disabled="isPredicting"
        >
          {{ isPredicting ? 'Processing...' : 'Prediction' }}
        </button>

        <div v-if="isPredicting" class="loading-overlay">
          <p>Analyzing X-Ray... Please wait.</p>
          <div class="spinner"></div>
        </div>

        <div class="image-preview" v-if="predictionImage && !isPredicting">
          <img :src="predictionImage" alt="Prediction Result" />
          <div class="file-info">
            <p><strong>Name Image:</strong> {{ fileName }}.png</p>
            <p><strong>Name Format:</strong> {{ fileName }}.xml</p>
          </div>
        </div>
      </div>

      <div class="action-group" v-if="predictionImage && !isPredicting">
        <button class="btn" @click="savePrediction">Save Prediction</button>
        <button class="btn" @click="downloadPrediction">Download Prediction</button>
      </div>
    </main>

    <footer class="footer">
      <p>&copy; {{ displayYear }} C303. Developed by Widi Arrohman at PENS.</p>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const fileInput = ref(null);
const rawImage = ref(null);
const predictionImage = ref(null);
const isPredicting = ref(false);
const fileName = ref('');

// Logic untuk Tahun Dinamis
const displayYear = computed(() => {
  const currentYear = new Date().getFullYear();
  if (currentYear > 2026) {
    return `2026-${currentYear}`;
  }
  return '2026';
});

const triggerUpload = () => {
  fileInput.value.click();
};

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  if (file) {
    fileName.value = `predict_boby_${new Date().getTime()}`;
    const reader = new FileReader();
    reader.onload = (e) => {
      rawImage.value = e.target.result;
      predictionImage.value = null; // Reset prediksi sebelumnya
    };
    reader.readAsDataURL(file);
  }
};

const startPrediction = () => {
  isPredicting.value = true;
  
  // Simulasi proses server selama 3 detik
  setTimeout(() => {
    isPredicting.value = false;
    // Menggunakan gambar yang sama sebagai contoh hasil prediksi
    predictionImage.value = rawImage.value;
    alert("Prediction Complete!");
  }, 3000);
};

const savePrediction = () => {
  alert("Data sent to Backend for Saving");
};

const downloadPrediction = () => {
  alert("Downloading result files...");
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

/* Header Styles */
.navbar {
  display: flex;
  align-items: center;
  padding: 10px 20px;
  border-bottom: 2px solid #000;
  gap: 20px;
}
.nav-links a {
  margin-right: 15px;
  text-decoration: none;
  color: black;
  padding: 5px 10px;
}
.nav-links a.active {
  background-color: #a0c4ff;
  border: 1px solid #000;
}

/* Content Styles */
.content {
  flex: 1;
  padding: 20px;
}
.section {
  margin-bottom: 30px;
}
.btn {
  background: white;
  border: 2px solid black;
  box-shadow: 3px 3px 0px black;
  padding: 8px 16px;
  cursor: pointer;
  font-weight: bold;
  margin-bottom: 15px;
  margin-right: 10px;
}
.btn:active {
  box-shadow: 1px 1px 0px black;
  transform: translate(2px, 2px);
}
.btn:disabled {
  background: #eee;
  cursor: not-allowed;
}

.image-preview {
  border: 1px solid #ccc;
  width: 100%;
  max-width: 600px;
  margin-top: 10px;
  background: #f9f9f9;
}
.image-preview img {
  width: 100%;
  display: block;
}

.file-info {
  margin-top: 10px;
  font-size: 0.9em;
}

/* Loading Spinner */
.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 10px 0;
}
@keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

/* Footer */
.footer {
  border-top: 1px solid #000;
  padding: 10px;
  text-align: center;
  font-size: 0.8em;
}
</style>