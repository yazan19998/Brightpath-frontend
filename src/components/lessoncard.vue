
<script setup>
defineProps({
  lesson: Object
})
const emit = defineEmits(['add-to-cart'])
</script>

<template>
  <div class="lesson-card">

    <div class="lesson-image">
      <span>{{ lesson.icon }}</span>
    </div>

    <div class="lesson-content">
      <h3>{{ lesson.subject }}</h3>

      <p class="location">
        📍 {{ lesson.location }}
      </p>

      <p class="spaces">
        {{ lesson.spaces }} spaces available
      </p>

      <div class="lesson-bottom">
        <strong>£{{ lesson.price }}</strong>

<button
  :disabled="lesson.spaces === 0"
  @click="emit('add-to-cart', lesson)"
>
  {{ lesson.spaces === 0 ? 'Full' : 'Add to Cart' }}
</button>

      </div>
    </div>

  </div>
</template>

<style scoped>
.lesson-card {
  background: white;
  border: 1px solid #e8e8e8;
  border-radius: 16px;
  overflow: hidden;
  max-width: 340px;
}

.lesson-image {
  background: #eaf0f7;
  height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 60px;
}

.lesson-content {
  padding: 22px;
}

h3 {
  color: #172b45;
  font-size: 22px;
  margin: 0 0 15px;
}

.location {
  color: #64748b;
}

.spaces {
  color: #23745b;
  font-size: 14px;
}

.lesson-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 25px;
}

strong {
  color: #172b45;
  font-size: 24px;
}

button {
  background: #172b45;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 12px 15px;
  cursor: pointer;
}

button:hover {
  background: #294769;
}

button:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
</style>
