
<script setup>
import { ref } from 'vue'

const lesson = ref({
  id: 1,
  subject: 'Mathematics',
  location: 'Hendon',
  price: 80,
  spaces: 5
})

const cartCount = ref(0)

function addToCart() {
  if (lesson.value.spaces > 0) {
    lesson.value.spaces--
    cartCount.value++
  }
}
</script>

<template>
  <header class="header">
    <h1>BrightPath Academy</h1>
    <span>Cart ({{ cartCount }})</span>
  </header>

  <main class="container">
    <div class="intro">
      <p class="eyebrow">EXPLORE & LEARN</p>
      <h2>Find your next adventure in learning.</h2>
      <p>
        Discover engaging after-school classes
        designed to inspire young minds.
      </p>
    </div>

    <section class="lesson-card">
      <div class="subject-icon">∑</div>

      <div class="lesson-info">
        <h3>{{ lesson.subject }}</h3>
        <p>Location: {{ lesson.location }}</p>
        <p>Available spaces: {{ lesson.spaces }}</p>
      </div>

      <div class="lesson-footer">
        <strong>£{{ lesson.price }}</strong>

        <button
          :disabled="lesson.spaces === 0"
          @click="addToCart"
        >
          {{ lesson.spaces === 0 ? 'Full' : 'Add to Cart' }}
        </button>
      </div>
    </section>
  </main>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f8f7f3;
  color: #172b45;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 8%;
  background: white;
  border-bottom: 1px solid #e5e7eb;
}

.header h1 {
  font-size: 22px;
}

.container {
  max-width: 1100px;
  margin: 70px auto;
  padding: 0 24px;
}

.intro {
  margin-bottom: 40px;
}

.eyebrow {
  color: #c57835;
  font-weight: bold;
  letter-spacing: 2px;
}

.intro h2 {
  font-size: clamp(28px, 4vw, 44px);
  max-width: 600px;
}

.intro p {
  line-height: 1.6;
}

.lesson-card {
  max-width: 350px;
  background: white;
  border: 1px solid #e9e9e9;
  border-radius: 16px;
  padding: 24px;
  box-shadow: 0 8px 24px #172b450d;
}

.subject-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 60px;
  height: 60px;
  background: #e6eef7;
  border-radius: 12px;
  font-size: 30px;
}

.lesson-info h3 {
  font-size: 22px;
}

.lesson-info p {
  color: #64748b;
}

.lesson-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 24px;
}

.lesson-footer strong {
  font-size: 25px;
}

button {
  background: #172b45;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 12px 18px;
  cursor: pointer;
}

button:disabled {
  opacity: 0.45;
  cursor: not-allowed;
}
</style>
