<template>
  <div class="skeleton-card">
    <div class="skeleton-header"></div>
    <div class="skeleton-image"></div>
    <div class="skeleton-stats"></div>
    <div class="skeleton-ability"></div>
    <div class="skeleton-ability"></div>
  </div>
</template>

<style scoped>
.skeleton-card {
  width: 300px;
  height: 460px;
  border: 12px solid #e0e0e0;
  border-radius: 12px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  background-color: #f5f5f5;
  box-sizing: border-box;
}

/* Animación de brillo/pulso */
.skeleton-header, .skeleton-image, .skeleton-stats, .skeleton-ability {
  background: linear-gradient(110deg, #ececec 8%, #f5f5f5 18%, #ececec 33%);
  background-size: 200% 100%;
  animation: 1.5s shine linear infinite;
  border-radius: 4px;
}

.skeleton-header { height: 24px; width: 100%; margin-bottom: 8px; }
.skeleton-image { height: 190px; width: 100%; margin-bottom: 10px; }
.skeleton-stats { height: 16px; width: 100%; margin-bottom: 20px; }
.skeleton-ability { height: 24px; width: 80%; margin: 5px auto; }

@keyframes shine {
  to { background-position-x: -200%; }
}
</style>