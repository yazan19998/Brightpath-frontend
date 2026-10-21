
<script setup>
import { ref, computed } from 'vue'
import LessonCard from './components/lessoncard.vue'
import { lessons as lessonData } from './data/lessons.js'

const lessons = ref(lessonData)


const cart = ref([])


const customerName = ref('')
const customerPhone = ref('')
const orderSubmitted = ref(false)

const customerEmail = ref('')
const customerAddress = ref('')
const customerCity = ref('')
const customerPostcode = ref('')
const customerCountry = ref('United Kingdom')
const studentName = ref('')




const showCart = ref(false)

function toggleCart() {
  showCart.value = !showCart.value
}


function addToCart(lesson) {
  if (lesson.spaces > 0) {
    cart.value.push(lesson.id)
    lesson.spaces--
  }
}



function removeFromCart(index) {
  const lessonId = cart.value[index]

  const lesson = lessons.value.find(
    lesson => lesson.id === lessonId
  )

  if (lesson) {
    lesson.spaces++
  }

  cart.value.splice(index, 1)
}




const validEmail = computed(() => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(customerEmail.value.trim())
})

const validAddress = computed(() => {
  return customerAddress.value.trim() !== '' &&
         customerCity.value.trim() !== '' &&
         customerPostcode.value.trim() !== '' &&
         customerCountry.value !== ''
})

const validStudent = computed(() => {
  return /^[A-Za-z ]+$/.test(studentName.value.trim())
})

const canCheckout = computed(() => {
  return validName.value &&
         validPhone.value &&
         validEmail.value &&
         validAddress.value &&
         validStudent.value &&
         cart.value.length > 0
})



function checkout() {
  if (!canCheckout.value) {
    return
  }

  orderSubmitted.value = true
}


const cartTotal = computed(() => {
  return cart.value.reduce((total, lessonId) => {
    const lesson = lessons.value.find(
      lesson => lesson.id === lessonId
    )

    return total + (lesson ? lesson.price : 0)
  }, 0)
})


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
    
<button
  class="cart-button"
  :disabled="cart.length === 0 && !showCart"
  @click="toggleCart"
>
  {{ showCart ? 'Back to Lessons' : 'Cart (' + cart.length + ')' }}
</button>

  </header>

  <main class="container">

    
<section v-if="showCart" class="cart-page">

  <h2>Your Shopping Cart</h2>
  <p>Review your selected lessons below.</p>

  <div v-if="cart.length === 0">
    <p>Your cart is empty.</p>
  </div>

  <div
    v-for="(lessonId, index) in cart"
    :key="index"
    class="cart-item"
  >
    <div v-if="lessons.find(lesson => lesson.id === lessonId)">
      <h3>
        {{ lessons.find(lesson => lesson.id === lessonId).subject }}
      </h3>

      <p>
        {{ lessons.find(lesson => lesson.id === lessonId).location }}
      </p>

      <strong>
        £{{ lessons.find(lesson => lesson.id === lessonId).price }}
      </strong>
    </div>

    <button class="remove-button" @click="removeFromCart(index)">
      Remove
    </button>
  </div>

  <div class="cart-total">
    <h3>Total</h3>
    <h2>£{{ cartTotal }}</h2>
  </div>

  

<div class="checkout-section">

  <h2>Complete Your Booking</h2>
  <p>Please enter your information below.</p>

  <h3>Parent / Guardian Information</h3>

  <div class="form-group">
    <label for="customerName">Full Name *</label>
    <input
      id="customerName"
      v-model="customerName"
      type="text"
      placeholder="Enter your full name"
    />
  </div>

  <div class="form-group">
    <label for="customerEmail">Email Address *</label>
    <input
      id="customerEmail"
      v-model="customerEmail"
      type="email"
      placeholder="example@email.com"
    />
  </div>

  <div class="form-group">
    <label for="customerPhone">Phone Number *</label>
    <input
      id="customerPhone"
      v-model="customerPhone"
      type="tel"
      placeholder="Enter your phone number"
    />
  </div>

  <h3>Address Information</h3>

  <div class="form-group">
    <label for="customerAddress">Street Address *</label>
    <input
      id="customerAddress"
      v-model="customerAddress"
      type="text"
      placeholder="House number and street name"
    />
  </div>

  <div class="form-row">
    <div class="form-group">
      <label for="customerCity">City / Location *</label>
      <input
        id="customerCity"
        v-model="customerCity"
        type="text"
        placeholder="Enter your city"
      />
    </div>

    <div class="form-group">
      <label for="customerPostcode">Postcode *</label>
      <input
        id="customerPostcode"
        v-model="customerPostcode"
        type="text"
        placeholder="Enter postcode"
      />
    </div>
  </div>

  <div class="form-group">
    <label for="customerCountry">Country *</label>
    <select id="customerCountry" v-model="customerCountry">
      <option value="United Kingdom">United Kingdom</option>
      <option value="United Arab Emirates">
        United Arab Emirates
      </option>
      <option value="Ireland">Ireland</option>
      <option value="Other">Other</option>
    </select>
  </div>

  <h3>Student Information</h3>

  <div class="form-group">
    <label for="studentName">Student Full Name *</label>
    <input
      id="studentName"
      v-model="studentName"
      type="text"
      placeholder="Enter student's full name"
    />
  </div>

  <button
    class="checkout-button"
    :disabled="!canCheckout || orderSubmitted"
    @click="checkout"
  >
    Confirm Booking
  </button>

  <p v-if="orderSubmitted" class="success-message">
    Your booking has been submitted successfully!
  </p>

</div>



</section>

<section v-else>

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


.cart-button {
  background: #172b45;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
}

.cart-button:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

.cart-page {
  max-width: 800px;
  margin: 0 auto;
}

.cart-page h2 {
  color: #172b45;
}

.cart-page > p {
  color: #64748b;
  margin-bottom: 30px;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  background: white;
  padding: 22px;
  margin-bottom: 15px;
  border: 1px solid #e8e8e8;
  border-radius: 12px;
}

.cart-item h3 {
  margin: 0 0 8px;
}

.cart-item p {
  color: #64748b;
}

.remove-button {
  background: #fff0ed;
  color: #bc4235;
  border: none;
  border-radius: 8px;
  padding: 10px 16px;
  cursor: pointer;
}

.remove-button:hover {
  background: #ffe0d9;
}

.cart-total {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 25px;
  padding: 20px 0;
  border-top: 1px solid #dce2e8;
}

@media (max-width: 550px) {
  .cart-item {
    flex-wrap: wrap;
  }
}


.checkout-section {
  background: white;
  padding: 30px;
  border: 1px solid #e8e8e8;
  border-radius: 16px;
  margin-top: 30px;
}

.checkout-section h2 {
  margin-top: 0;
  color: #172b45;
}

.checkout-section p {
  color: #64748b;
}

.form-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

.form-group label {
  font-weight: bold;
  margin-bottom: 8px;
}

.form-group input {
  padding: 14px;
  border: 1px solid #dce2e8;
  border-radius: 8px;
  font-size: 15px;
}

.form-group input:focus {
  outline: 2px solid #c57835;
}

.checkout-button {
  width: 100%;
  background: #172b45;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 15px;
  font-size: 16px;
  cursor: pointer;
}

.checkout-button:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

.success-message {
  margin-top: 20px;
  padding: 15px;
  background: #e8f7ef;
  color: #23745b !important;
  border-radius: 8px;
}


.checkout-section h3 {
  color: #172b45;
  font-size: 18px;
  margin-top: 30px;
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 1px solid #e8e8e8;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.form-group select {
  padding: 14px;
  border: 1px solid #dce2e8;
  border-radius: 8px;
  background: white;
  color: #172b45;
  font-size: 15px;
}

.form-group input,
.form-group select {
  width: 100%;
}

@media (max-width: 600px) {
  .form-row {
    grid-template-columns: 1fr;
  }

  .checkout-section {
    padding: 20px;
  }
}



</style>
