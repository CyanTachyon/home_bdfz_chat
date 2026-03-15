<script setup>
const props = defineProps({
  url: String,
  title: String,
  description: String,
  icon: { type: String, default: null },
  isCurrent: { type: Boolean, default: false },
  index: { type: Number, default: 0 }
})

const avatarColors = [
  ['#6366f1', '#8b5cf6'],
  ['#8b5cf6', '#ec4899'],
  ['#06b6d4', '#3b82f6'],
  ['#10b981', '#06b6d4'],
  ['#f59e0b', '#ef4444'],
  ['#ec4899', '#f43f5e'],
  ['#14b8a6', '#22d3ee'],
]

const colorPair = avatarColors[props.index % avatarColors.length]
const avatarGradient = `linear-gradient(135deg, ${colorPair[0]}, ${colorPair[1]})`

function onImgError(e) {
  e.target.style.display = 'none'
  const parent = e.target.parentElement
  if (parent) {
    const fallback = document.createElement('span')
    fallback.className = 'icon-letter'
    fallback.textContent = props.title.charAt(0)
    parent.appendChild(fallback)
  }
}
</script>

<template>
  <a
    :href="url"
    class="link-card"
    :class="{ 'is-current': isCurrent }"
    :target="isCurrent ? '_self' : '_blank'"
    :rel="isCurrent ? undefined : 'noopener noreferrer'"
    :aria-label="`${title} — ${description}`"
    :style="{ animationDelay: `${index * 0.05}s` }"
  >
    <div class="card-icon" :style="{ background: avatarGradient }">
      <img
        v-if="icon"
        :src="icon"
        :alt="title"
        class="icon-img"
        @error="onImgError"
      />
      <span v-else class="icon-letter">{{ title.charAt(0) }}</span>
    </div>
    <div class="card-content">
      <h3 class="card-title">
        {{ title }}
        <span v-if="isCurrent" class="current-badge">当前</span>
      </h3>
      <p class="card-desc">{{ description }}</p>
    </div>
  </a>
</template>

<style scoped>
.link-card {
  display: flex;
  align-items: flex-start;
  gap: var(--space-md);
  padding: var(--space-lg);
  background: var(--surface);
  backdrop-filter: blur(16px) saturate(150%) brightness(1.05);
  -webkit-backdrop-filter: blur(16px) saturate(150%) brightness(1.05);
  border: 1px solid var(--border);
  border-radius: var(--radius-lg);
  box-shadow:
    var(--shadow-card),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
  transition:
    transform var(--duration-fast) var(--ease-out),
    box-shadow var(--duration-fast) var(--ease-out),
    background var(--duration-fast) var(--ease-out),
    border-color var(--duration-fast) var(--ease-out);
  animation: fadeInUp 0.5s var(--ease-out) both;
  cursor: pointer;
}

.link-card:hover {
  transform: translateY(-4px);
  box-shadow:
    var(--shadow-card-hover),
    inset 0 1px 0 rgba(255, 255, 255, 0.08);
  background: var(--surface-hover);
  border-color: var(--border-hover);
}

.link-card:focus-visible {
  outline: 2px solid var(--accent-1);
  outline-offset: 2px;
}

.is-current {
  border-color: rgba(99, 102, 241, 0.35);
  background: rgba(99, 102, 241, 0.06);
  box-shadow:
    var(--shadow-card),
    inset 0 1px 0 rgba(255, 255, 255, 0.05),
    0 0 20px rgba(99, 102, 241, 0.08);
}

.is-current:hover {
  border-color: rgba(99, 102, 241, 0.5);
  background: rgba(99, 102, 241, 0.1);
  box-shadow:
    var(--shadow-card-hover),
    inset 0 1px 0 rgba(255, 255, 255, 0.08),
    0 0 30px rgba(99, 102, 241, 0.12);
}

.card-icon {
  flex-shrink: 0;
  width: 48px;
  height: 48px;
  border-radius: var(--radius-md);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow:
    0 2px 8px rgba(0, 0, 0, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.15);
  transition: transform var(--duration-fast) var(--ease-out);
}

.link-card:hover .card-icon {
  transform: scale(1.05);
}

.icon-img {
  width: 32px;
  height: 32px;
  border-radius: var(--radius-sm);
  object-fit: cover;
}

.icon-letter {
  color: #fff;
  font-weight: 700;
  font-size: 1.2rem;
  line-height: 1;
  user-select: none;
}

.card-content {
  min-width: 0;
  flex: 1;
}

.card-title {
  font-size: 1rem;
  font-weight: 600;
  color: var(--text-primary);
  line-height: 1.4;
  display: flex;
  align-items: center;
  gap: var(--space-xs);
}

.current-badge {
  display: inline-block;
  font-size: 0.7rem;
  font-weight: 600;
  padding: 2px 8px;
  border-radius: 9999px;
  background: var(--accent-1);
  color: #fff;
  line-height: 1.4;
  flex-shrink: 0;
}

.card-desc {
  font-size: 0.85rem;
  color: var(--text-secondary);
  line-height: 1.4;
  margin-top: 4px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>
