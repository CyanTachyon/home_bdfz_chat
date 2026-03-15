<script setup>
</script>

<template>
  <header class="nav-header">
    <div class="header-glow"></div>
    <h1 class="site-title">
      <span class="title-gradient">bdfz.chat</span>
    </h1>
    <p class="site-subtitle">服务导航</p>
    <div class="header-divider"></div>
  </header>
</template>

<style scoped>
.nav-header {
  text-align: center;
  padding: var(--space-2xl) 0 var(--space-xl);
  animation: fadeInUp 0.6s var(--ease-out) both;
  position: relative;
}

.header-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
  height: 120px;
  background: radial-gradient(ellipse, rgba(99, 102, 241, 0.12), transparent 70%);
  pointer-events: none;
  animation: subtleGlow 4s ease-in-out infinite;
}

.site-title {
  font-size: clamp(2.5rem, 6vw, 3.5rem);
  font-weight: 800;
  line-height: 1.1;
  position: relative;
}

.title-gradient {
  background: linear-gradient(135deg, var(--accent-1), var(--accent-2), var(--accent-3));
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.site-subtitle {
  color: var(--text-secondary);
  font-size: 1.1rem;
  margin-top: var(--space-sm);
  letter-spacing: 0.1em;
  font-weight: 300;
}

.header-divider {
  width: 48px;
  height: 3px;
  background: linear-gradient(90deg, var(--accent-1), var(--accent-3));
  border-radius: 2px;
  margin: var(--space-lg) auto 0;
  opacity: 0.5;
}
</style>
