<script setup>
import LinkCard from './LinkCard.vue'

defineProps({
  title: String,
  description: { type: String, default: '' },
  links: Array
})
</script>

<template>
  <section class="category-section">
    <div class="section-header">
      <h2 class="section-title">{{ title }}</h2>
      <p v-if="description" class="section-desc">{{ description }}</p>
    </div>
    <div class="section-grid">
      <LinkCard
        v-for="(link, index) in links"
        :key="link.url"
        :url="link.url"
        :title="link.title"
        :description="link.description"
        :icon="link.icon"
        :is-current="link.isCurrent"
        :index="index"
      />
    </div>
  </section>
</template>

<style scoped>
.category-section {
  margin-bottom: var(--space-2xl);
}

.category-section + .category-section {
  padding-top: var(--space-lg);
  border-top: 1px solid rgba(255, 255, 255, 0.04);
}

.section-header {
  margin-bottom: var(--space-lg);
  animation: fadeInUp 0.5s var(--ease-out) both;
}

.section-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--text-primary);
  line-height: 1.3;
  position: relative;
  display: inline-block;
}

.section-title::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, var(--accent-1), var(--accent-3), transparent);
  border-radius: 1px;
  opacity: 0.6;
}

.section-desc {
  font-size: 0.9rem;
  color: var(--text-secondary);
  margin-top: var(--space-sm);
}

.section-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(100%, 280px), 1fr));
  gap: clamp(16px, 3vw, 24px);
}
</style>
