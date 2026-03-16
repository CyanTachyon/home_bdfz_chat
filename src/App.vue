<script setup>
import { ref, computed, onMounted } from 'vue'
import { categories } from './data/links.js'
import NavHeader from './components/NavHeader.vue'
import CategorySection from './components/CategorySection.vue'

const blogLinks = ref([])
const blogLoading = ref(true)
const activeFilter = ref(null)

const allCategories = computed(() => {
  const list = [...categories]
  if (!blogLoading.value && blogLinks.value.length) {
    list.push({
      id: 'student-blog',
      title: '学生博客',
      description: '来自 bdfz.chat 的学生博客',
      links: blogLinks.value
    })
  }
  return list
})

const filteredCategories = computed(() => {
  if (!activeFilter.value) return allCategories.value
  return allCategories.value.filter(c => c.id === activeFilter.value)
})

function toggleFilter(id) {
  activeFilter.value = activeFilter.value === id ? null : id
}

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
    <nav class="filter-bar">
      <button
        class="filter-bubble"
        :class="{ active: !activeFilter }"
        @click="activeFilter = null"
      >
        全部
      </button>
      <button
        v-for="category in allCategories"
        :key="category.id"
        class="filter-bubble"
        :class="{ active: activeFilter === category.id }"
        @click="toggleFilter(category.id)"
      >
        {{ category.title }}
      </button>
    </nav>
    <main class="main-content">
      <CategorySection
        v-for="category in filteredCategories"
        :key="category.id"
        :title="category.title"
        :description="category.description"
        :links="category.links"
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

/* ── Filter Bubbles ── */
.filter-bar {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-sm);
  justify-content: center;
  padding: var(--space-md) 0 var(--space-lg);
  animation: fadeInUp 0.5s var(--ease-out) 0.2s both;
}

.filter-bubble {
  position: relative;
  padding: 8px 20px;
  font-size: 0.875rem;
  font-weight: 500;
  font-family: inherit;
  line-height: 1.4;
  color: var(--text-secondary);
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 9999px;
  cursor: pointer;
  transition:
    color var(--duration-fast) var(--ease-out),
    background var(--duration-fast) var(--ease-out),
    border-color var(--duration-fast) var(--ease-out),
    box-shadow var(--duration-fast) var(--ease-out),
    transform var(--duration-fast) var(--ease-out);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  user-select: none;
  white-space: nowrap;
}

.filter-bubble:hover {
  color: var(--text-primary);
  background: var(--surface-hover);
  border-color: var(--border-hover);
  transform: translateY(-1px);
  box-shadow:
    0 4px 12px rgba(99, 102, 241, 0.08),
    inset 0 1px 0 rgba(255, 255, 255, 0.04);
}

.filter-bubble:focus-visible {
  outline: 2px solid var(--accent-1);
  outline-offset: 2px;
}

.filter-bubble.active {
  color: #fff;
  background: linear-gradient(135deg, var(--accent-1), var(--accent-2));
  border-color: transparent;
  box-shadow:
    0 4px 16px rgba(99, 102, 241, 0.3),
    0 0 0 1px rgba(139, 92, 246, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.12);
  transform: translateY(-1px);
}

.filter-bubble.active:hover {
  box-shadow:
    0 6px 20px rgba(99, 102, 241, 0.4),
    0 0 0 1px rgba(139, 92, 246, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.12);
}

@media (max-width: 480px) {
  .filter-bar {
    gap: var(--space-xs);
    padding: var(--space-sm) 0 var(--space-md);
  }

  .filter-bubble {
    padding: 6px 14px;
    font-size: 0.8125rem;
  }
}
</style>
