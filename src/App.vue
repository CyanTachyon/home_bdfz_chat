<script setup>
import { categories } from './data/links.js'
import NavHeader from './components/NavHeader.vue'
import CategorySection from './components/CategorySection.vue'
</script>

<template>
  <div class="app-container">
    <NavHeader />
    <main class="main-content">
      <CategorySection
        v-for="category in categories"
        :key="category.id"
        :title="category.title"
        :description="category.description"
        :links="category.links"
      />
    </main>
    <footer class="site-footer">
      <p>bdfz.chat &copy; {{ new Date().getFullYear() }}</p>
    </footer>
  </div>
</template>

<style scoped>
.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 clamp(16px, 4vw, 48px);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.main-content {
  flex: 1;
}

.site-footer {
  text-align: center;
  color: var(--text-secondary);
  font-size: 0.85rem;
  padding: var(--space-xl) 0;
  border-top: 1px solid rgba(255, 255, 255, 0.04);
  margin-top: var(--space-xl);
  animation: fadeIn 0.8s var(--ease-out) 0.5s both;
}
</style>
