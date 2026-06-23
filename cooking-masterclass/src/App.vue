<script setup>
import { ref } from 'vue'

const courses = ref([
  { id: 1, title: 'Italian Pasta Basics', chef: 'Chef Marco Rossi', level: 'Beginner', price: 1200, available: true, img: 'https://images.unsplash.com/photo-1551183053-bf91a1d81141?q=80&w=800&auto=format&fit=crop' },
  { id: 2, title: 'Sourdough Masterclass', chef: 'Chef Lina Park', level: 'Advanced', price: 1800, available: false, img: 'https://images.unsplash.com/photo-1549931319-a545dcf3bc73?q=80&w=800&auto=format&fit=crop' },
  { id: 3, title: 'Sushi Rolling 101', chef: 'Chef Kenji Tanaka', level: 'Intermediate', price: 1500, available: true, img: 'https://images.unsplash.com/photo-1611143669185-af224c5e3252?q=80&w=800&auto=format&fit=crop' },
  { id: 4, title: 'Decadent Desserts', chef: 'Chef Amara Singh', level: 'Beginner', price: 1350, available: true, img: 'https://images.unsplash.com/photo-1578985545062-69928b1d9587?q=80&w=800&auto=format&fit=crop' }
])

const wishlist = ref([])

function toggleWishlist(course) {
  const index = wishlist.value.findIndex(c => c.id === course.id)
  if (index === -1) {
    wishlist.value.push(course)
  } else {
    wishlist.value.splice(index, 1)
  }
}

function isSaved(course) {
  return wishlist.value.some(c => c.id === course.id)
}
</script>

<template>
  <div class="app">
    <header class="header">
      <h1>🍳 Cooking Masterclass</h1>
      <div class="wishlist-count"> {{ wishlist.length }}</div>
    </header>

    <main class="catalogue">
      <div v-for="course in courses" :key="course.id" class="card" :class="{ 'sold-out': !course.available }">
        <img :src="course.img" :alt="course.title" loading="lazy" />
        <div class="card-body">
          <h2>{{ course.title }}</h2>
          <p class="chef">by {{ course.chef }}</p>
          <div class="meta">
            <span class="level">{{ course.level }}</span>
            <span class="price">R{{ course.price.toLocaleString() }}</span>
          </div>
          <span v-if="!course.available" class="badge">SOLD OUT</span>
          <button v-else @click="toggleWishlist(course)" :class="{ saved: isSaved(course) }">
            {{ isSaved(course) ? 'Saved ✓' : 'Save' }}
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
:root {
  --orange: #01ffaf;
  --cream: #FAF3E0;
  --dark: #264653;
}
.app { background: var(--cream); min-height: 100vh; font-family: 'Segoe UI', sans-serif; color: var(--dark); }
.header { display: flex; justify-content: space-between; align-items: center; padding: 1.5rem 2rem; background: white; box-shadow: 0 2px 8px rgba(0,0,0,0.05); position: sticky; top: 0; z-index: 10; }
.header h1 { font-size: 1.5rem; color: var(--orange); margin: 0; }
.wishlist-count { font-weight: 700; font-size: 1.2rem; }
.catalogue { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 2rem; padding: 2rem; max-width: 1200px; margin: 0 auto; }
.card { background: rgb(230, 23, 23); border-radius: 16px; overflow: hidden; box-shadow: 0 4px 12px rgba(0,0,0,0.08); transition: transform 0.2s; }
.card:hover { transform: translateY(-8px); }
.card.sold-out { opacity: 0.6; }
.card img { width: 100%; height: 180px; object-fit: cover; display: block; }
.card-body { padding: 1.2rem; }
.card-body h2 { margin: 0 0 0.3rem; font-size: 1.1rem; }
.chef { color: #666; font-size: 0.9rem; margin: 0 0 1rem; }
.meta { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem; }
.level { background: var(--cream); padding: 0.3rem 0.8rem; border-radius: 20px; font-size: 0.8rem; font-weight: 600; }
.price { font-weight: 700; font-size: 1.1rem; color: var(--orange); }
.badge { display: block; text-align: center; background: #999; color: rgb(0, 0, 0); padding: 0.7rem; border-radius: 8px; font-weight: 700; }
button { width: 100%; padding: 0.7rem; border: none; border-radius: 8px; background: var(--orange); color: white; font-weight: 700; cursor: pointer; transition: background 0.2s; }
button:hover { background: #d65a3a; }
button.saved { background: #01fd16; }
@media (max-width: 450px) { .header { padding: 1rem; } .catalogue { padding: 1rem; gap: 1rem; } }
</style>