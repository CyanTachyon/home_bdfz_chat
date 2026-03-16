<script setup>
import { ref, onMounted } from 'vue'
import { categories } from './data/links.js'
import NavHeader from './components/NavHeader.vue'
import CategorySection from './components/CategorySection.vue'

const blogLinks = ref([])
const blogLoading = ref(true)

onMounted(async () => {
  try {
    const res = await fetch('https://app.bdfz.chat/api/public-sites')
    const data = await res.json()
    blogLinks.value = (data.sites || []).map(site => ({
      url: site.url,
      title: site.displayName,
      description: site.description || site.slug,
      icon: null,
      isCurrent: false
    }))
  } catch {
    blogLinks.value = []
  } finally {
    blogLoading.value = false
  }
})
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
      <CategorySection
        v-if="!blogLoading && blogLinks.length"
        title="学生博客"
        description="来自 bdfz.chat 的学生博客"
        :links="blogLinks"
      />
      <section v-if="blogLoading" class="loading-section">
        <h2 class="loading-title">学生博客</h2>
        <div class="loading-indicator">
          <span class="loading-dot"></span>
          <span class="loading-dot"></span>
          <span class="loading-dot"></span>
        </div>
      </section>
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

.loading-section {
  margin-bottom: var(--space-2xl);
  animation: fadeInUp 0.5s var(--ease-out) both;
}

.loading-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: var(--space-lg);
}

.loading-indicator {
  display: flex;
  gap: 6px;
  padding: var(--space-lg) 0;
}

.loading-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--accent-1);
  opacity: 0.4;
  animation: loadPulse 1.2s ease-in-out infinite;
}

.loading-dot:nth-child(2) { animation-delay: 0.15s; }
.loading-dot:nth-child(3) { animation-delay: 0.3s; }

@keyframes loadPulse {
  0%, 100% { opacity: 0.4; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.2); }
}
</style>
