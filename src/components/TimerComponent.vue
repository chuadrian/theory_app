// src/components/TimerComponent.vue
<template>
  <h2>Time Remaining: {{ formatTime(timeRemaining) }}</h2>
</template>

<script>
export default {
  name: 'TimerComponent',
  props: {
    timeRemaining: {
      type: Number,
      required: true
    }
  },
  methods: {
    formatTime(seconds) {
      const minutes = Math.floor(seconds / 60);
      const remainingSeconds = seconds % 60;
      return `${minutes}:${remainingSeconds.toString().padStart(2, '0')}`;
    }
  }
};
</script>

<style scoped>
.timer {
  background-color: #ffd700;
  padding: 10px;
  border-radius: 10px;
  text-align: center;
  margin-bottom: 20px;
  animation: pulse 2s infinite;
}

.timer h2 {
  margin: 0;
  color: #8b4513;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}
</style>