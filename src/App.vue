
<script setup>
import { ref, computed } from 'vue'
import LessonCard from './components/lessoncard.vue'
import { lessons as lessonData } from './data/lessons.js'

const lessons = ref(lessonData)


const cart = ref([])

function addToCart(lesson) {
  if (lesson.spaces > 0) {
    cart.value.push(lesson.id)
    lesson.spaces--
  }
}


const sortBy = ref('subject')
const sortOrder = ref('asc')

const sortedLessons = computed(() => {
  return [...lessons.value].sort((a, b) => {
    let first = a[sortBy.value]
    let second = b[sortBy.value]

    if (typeof first === 'string') {
      first = first.toLowerCase()
      second = second.toLowerCase()
    }

    if (first < second) {
      return sortOrder.value === 'asc' ? -1 : 1
    }

    if (first > second) {
      return sortOrder.value === 'asc' ? 1 : -1
    }

    return 0
  })
})
</script>


<template>
  <header class="header">
    <h1>BrightPath Academy</h1>
    <span>Cart ({{ cart.length }})</span>
  </header>

  <main class="container">
    <section class="intro">
      <p class="eyebrow">EXPLORE & LEARN</p>
      <h2>Discover your next adventure in learning.</h2>
      <p>Find exciting after-school activities for young minds.</p>
    </section>

    <h2>Explore Our Lessons</h2>

    
<div class="sort-controls">
  <label for="sortBy">Sort by:</label>

  <select id="sortBy" v-model="sortBy">
    <option value="subject">Subject</option>
    <option value="location">Location</option>
    <option value="price">Price</option>
    <option value="spaces">Available Spaces</option>
  </select>

  <select v-model="sortOrder" aria-label="Sort direction">
    <option value="asc">Ascending</option>
    <option value="desc">Descending</option>
  </select>
</div>



<div class="lesson-grid">
  <LessonCard
    v-for="lesson in sortedLessons"
    :key="lesson.id"
    :lesson="lesson"
    @add-to-cart="addToCart"
  />
</div>


    

<div class="lesson-grid">
  <LessonCard
    v-for="lesson in sortedLessons"
    :key="lesson.id"
    :lesson="lesson"
    @add-to-cart="addToCart"
  />
</div>


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

.container {
  max-width: 1100px;
  margin: 65px auto;
  padding: 0 24px;
}

.intro {
  margin-bottom: 50px;
}

.eyebrow {
  color: #c57835;
  font-weight: bold;
  letter-spacing: 2px;
}

.intro h2 {
  font-size: clamp(28px, 4vw, 44px);
  max-width: 650px;
}

.intro p {
  color: #64748b;
}

.lesson-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 24px;
  margin-top: 25px;
}

@media (max-width: 800px) {
  .lesson-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 550px) {
  .lesson-grid {
    grid-template-columns: 1fr;
  }
}


.sort-controls {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: 25px 0;
  flex-wrap: wrap;
}

.sort-controls label {
  font-weight: bold;
  color: #172b45;
}

.sort-controls select {
  padding: 12px 16px;
  border: 1px solid #dce2e8;
  border-radius: 8px;
  background: white;
  color: #172b45;
  font-size: 14px;
  cursor: pointer;
}

.sort-controls select:focus {
  outline: 2px solid #c57835;
}


</style>
