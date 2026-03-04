<template>
  <div class="app-container">
    <!-- <header class="navbar">
      <div class="logo">
        <img src="https://via.placeholder.com/40" alt="Logo" />
      </div>
      <nav class="nav-links">
        <a href="#" class="active">Home</a>
        <a href="#">Predict</a>
        <a href="#">History</a>
      </nav>
    </header> -->

    <main class="content">
      <div class="slider-container">
        <div class="slider-wrapper">
          <div 
            v-for="(image, index) in slides" 
            :key="index" 
            class="slide"
            v-show="currentSlide === index"
          >
            <img :src="image" alt="Dental Radiograph Analysis" />
            
            <div class="overlay-content">
              <h1>Bone Loss Severity and Tooth Numbering</h1>
              <p>Upload a panoramic radiograph to analyze bone loss severity and detect tooth numbering automatically.</p>
            </div>
          </div>

          <button class="arrow prev" @click="prevSlide">&#10094;</button>
          <button class="arrow next" @click="nextSlide">&#10095;</button>

          <div class="dots-container">
            <span 
              v-for="(dot, index) in slides" 
              :key="index" 
              class="dot" 
              :class="{ active: currentSlide === index }"
              @click="currentSlide = index"
            ></span>
          </div>
        </div>
      </div>
    </main>

    <footer class="footer">
      <p>&copy; {{ displayYear }} C303. Developed by Widi Arrohman at PENS.</p>
    </footer>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted, onUnmounted, computed } from 'vue';


// List Gambar Slider (Ganti URL dengan gambar asli Anda)
const slides = ref([
  'https://images.unsplash.com/photo-1588776814546-1ffcf47267a5?auto=format&fit=crop&w=1200&q=80',
  'https://images.unsplash.com/photo-1606811841689-23dfddce3e95?auto=format&fit=crop&w=1200&q=80',
  'https://images.unsplash.com/photo-1598256989800-fe5f95da9787?auto=format&fit=crop&w=1200&q=80',
  'https://images.unsplash.com/photo-1579684385127-1ef15d508118?auto=format&fit=crop&w=1200&q=80',
  'https://images.unsplash.com/photo-1576091160550-2173dba999ef?auto=format&fit=crop&w=1200&q=80'
]);

const currentSlide = ref(0);
let timer = null;

// Logic Navigasi
const nextSlide = () => {
  currentSlide.value = (currentSlide.value + 1) % slides.value.length;
};

const prevSlide = () => {
  currentSlide.value = (currentSlide.value - 1 + slides.value.length) % slides.value.length;
};

// Autoplay 3 detik
const startTimer = () => {
  timer = setInterval(nextSlide, 3000);
};

const stopTimer = () => {
  clearInterval(timer);
};

onMounted(() => {
  startTimer();
});

onUnmounted(() => {
  stopTimer();
});

// Logic Tahun Dinamis
const displayYear = computed(() => {
  const currentYear = new Date().getFullYear();
  return currentYear > 2026 ? `2026-${currentYear}` : '2026';
});

const predict = async () => {
  try {
    const response = await axios.post('/api/predict', { /* data yang diperlukan */ });
    console.log('Prediction Result:', response.data);
  } catch (error) {
    console.error('Error during prediction:', error);
  }
};

</script>

<style scoped>
.app-container {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  max-width: 900px;
  margin: 0 auto;
  border: 2px solid #000;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #fff;
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
  font-weight: 500;
}
.nav-links a.active {
  background-color: #a0c4ff;
  border: 1px solid #000;
}

/* Content */
.content {
  flex: 1;
  padding: 0; /* Slider menempel ke pinggir */
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Slider */
.slider-container {
  width: 100%;
  padding: 20px;
}

.slider-wrapper {
  position: relative;
  width: 100%;
  height: 450px;
  border: 3px solid #000;
  overflow: hidden;
  background-color: #333;
}

.slide {
  width: 100%;
  height: 100%;
  position: relative;
}

.slide img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.6; /* Membuat teks lebih terbaca */
}

/* Overlay Text */
.overlay-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: white;
  width: 80%;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
}

.overlay-content h1 {
  font-size: 2rem;
  margin-bottom: 10px;
}

.overlay-content p {
  font-size: 1.1rem;
  line-height: 1.4;
}

/* Navigation Buttons */
.arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: white;
  border: 2px solid #000;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  font-size: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
  box-shadow: 2px 2px 0px #000;
}
.arrow:active { transform: translateY(-50%) translate(1px, 1px); box-shadow: 0px 0px 0px #000; }
.prev { left: 15px; }
.next { right: 15px; }

/* Dots Indicator */
.dots-container {
  position: absolute;
  bottom: 20px;
  width: 100%;
  display: flex;
  justify-content: center;
  gap: 10px;
}
.dot {
  width: 15px;
  height: 15px;
  background-color: #fff;
  border: 2px solid #000;
  border-radius: 50%;
  cursor: pointer;
}
.dot.active {
  background-color: #000;
}

/* Footer */
.footer {
  border-top: 2px solid #000;
  padding: 15px;
  text-align: center;
  font-size: 0.9em;
}
</style>