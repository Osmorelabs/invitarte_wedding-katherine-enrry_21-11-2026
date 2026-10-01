<template>
  <div class="countdown-widget">
    <div class="countdown-header">FALTAN PARA EL GRAN DÍA</div>
    <div class="timer-grid">
      <div class="timer-box">
        <span class="timer-value">{{ days }}</span>
        <span class="timer-label">DÍAS</span>
      </div>
      <div class="timer-colon">:</div>
      <div class="timer-box">
        <span class="timer-value">{{ formatTwoDigits(hours) }}</span>
        <span class="timer-label">HORAS</span>
      </div>
      <div class="timer-colon">:</div>
      <div class="timer-box">
        <span class="timer-value">{{ formatTwoDigits(minutes) }}</span>
        <span class="timer-label">MIN</span>
      </div>
      <div class="timer-colon">:</div>
      <div class="timer-box">
        <span class="timer-value">{{ formatTwoDigits(seconds) }}</span>
        <span class="timer-label">SEG</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const targetDate = new Date('2026-11-21T12:00:00');

const days = ref(0);
const hours = ref(0);
const minutes = ref(0);
const seconds = ref(0);
let timerInterval = null;

function updateCountdown() {
  const now = new Date();
  const diff = targetDate.getTime() - now.getTime();

  if (diff <= 0) {
    days.value = 0;
    hours.value = 0;
    minutes.value = 0;
    seconds.value = 0;
    return;
  }

  days.value = Math.floor(diff / (1000 * 60 * 60 * 24));
  hours.value = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  minutes.value = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
  seconds.value = Math.floor((diff % (1000 * 60)) / 1000);
}

function formatTwoDigits(n) {
  return n.toString().padStart(2, '0');
}

onMounted(() => {
  updateCountdown();
  timerInterval = setInterval(updateCountdown, 1000);
});

onUnmounted(() => {
  if (timerInterval) clearInterval(timerInterval);
});
</script>

<style scoped>
.countdown-widget {
  margin: 1.5rem 0;
  padding: 1.1rem 0.6rem;
  background: rgba(240, 199, 197, 0.38);
  border-radius: 14px;
  border: 1.5px solid rgba(226, 142, 140, 0.6);
  text-align: center;
  box-shadow: 0 4px 15px rgba(240, 199, 197, 0.25);
}

.countdown-header {
  font-family: var(--font-sans);
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  color: #9E4B49;
  margin-bottom: 0.75rem;
  text-transform: uppercase;
}

.timer-grid {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.35rem;
}

.timer-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-width: 50px;
  padding: 0.5rem 0.3rem;
  background: var(--color-white, #FFFFFF);
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.06);
  border: 1px solid rgba(226, 142, 140, 0.4);
}

.timer-value {
  font-family: var(--font-serif);
  font-size: 1.35rem;
  font-weight: 700;
  color: #9E4B49;
  line-height: 1;
}

.timer-label {
  font-family: var(--font-sans);
  font-size: 0.55rem;
  font-weight: 700;
  color: var(--color-text-muted, #8C7565);
  margin-top: 0.25rem;
  letter-spacing: 0.08em;
}

.timer-colon {
  font-family: var(--font-serif);
  font-size: 1.2rem;
  font-weight: bold;
  color: var(--color-gold, #7F6000);
}
</style>
