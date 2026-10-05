<template>
  <!-- Floating mute/play button (teleported to body so it stays fixed while scrolling) -->
  <Teleport to="body" v-if="isMounted">
    <button
      class="floating-mute-btn"
      :class="{ muted: isMuted || !isPlaying }"
      @click="onFloatingClick"
      :aria-label="isPlaying && !isMuted ? 'Silenciar música' : 'Reproducir música'"
    >
      <span v-if="isPlaying && !isMuted" class="pulse-ring" aria-hidden="true"></span>
      <svg v-if="isPlaying && !isMuted" viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5" fill="currentColor" />
        <path d="M15.54 8.46a5 5 0 0 1 0 7.07" /><path d="M19.07 4.93a10 10 0 0 1 0 14.14" />
      </svg>
      <svg v-else viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5" fill="currentColor" />
        <line x1="23" y1="9" x2="17" y2="15" /><line x1="17" y1="9" x2="23" y2="15" />
      </svg>
    </button>
  </Teleport>

  <div class="music-player" @dblclick="togglePlay">
    <audio
      ref="audioEl"
      :src="src"
      preload="auto"
      :loop="isLooping"
      @loadedmetadata="onLoaded"
      @timeupdate="onTimeUpdate"
      @play="isPlaying = true"
      @pause="isPlaying = false"
      @ended="onEnded"
    ></audio>

    <p class="player-hint">DOBLE CLICK PARA REPRODUCIR</p>

    <div class="progress-row">
      <span class="time">{{ formatTime(currentTime) }}</span>
      <div
        class="progress-track"
        ref="trackEl"
        @click.stop="seek"
        @dblclick.stop
        role="slider"
        aria-label="Progreso de la canción"
        :aria-valuenow="Math.round(currentTime)"
        :aria-valuemax="Math.round(duration)"
      >
        <div class="progress-fill" :style="{ width: progress + '%' }"></div>
        <div class="progress-thumb" :style="{ left: progress + '%' }"></div>
      </div>
      <span class="time">{{ formatTime(duration) }}</span>
    </div>

    <div class="controls" @dblclick.stop>
      <!-- Repetir -->
      <button class="ctrl small" :class="{ active: isLooping }" @click="isLooping = !isLooping" aria-label="Repetir">
        <svg viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polyline points="17 1 21 5 17 9" /><path d="M3 11V9a4 4 0 0 1 4-4h14" />
          <polyline points="7 23 3 19 7 15" /><path d="M21 13v2a4 4 0 0 1-4 4H3" />
        </svg>
      </button>

      <!-- Retroceder -->
      <button class="ctrl" @click="skip(-10)" aria-label="Retroceder 10 segundos">
        <svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor">
          <rect x="4" y="5" width="2.5" height="14" /><polygon points="20,5 20,19 8,12" />
        </svg>
      </button>

      <!-- Play / Pausa -->
      <button class="ctrl play" @click="togglePlay" :aria-label="isPlaying ? 'Pausar' : 'Reproducir'">
        <svg v-if="!isPlaying" viewBox="0 0 24 24" width="20" height="20" fill="currentColor">
          <polygon points="7,4 20,12 7,20" />
        </svg>
        <svg v-else viewBox="0 0 24 24" width="20" height="20" fill="currentColor">
          <rect x="6" y="4" width="4" height="16" /><rect x="14" y="4" width="4" height="16" />
        </svg>
      </button>

      <!-- Adelantar -->
      <button class="ctrl" @click="skip(10)" aria-label="Adelantar 10 segundos">
        <svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor">
          <polygon points="4,5 4,19 16,12" /><rect x="17.5" y="5" width="2.5" height="14" />
        </svg>
      </button>

      <!-- Silenciar -->
      <button class="ctrl small" :class="{ active: isMuted }" @click="toggleMute" :aria-label="isMuted ? 'Activar sonido' : 'Silenciar'">
        <svg v-if="!isMuted" viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5" fill="currentColor" />
          <path d="M15.54 8.46a5 5 0 0 1 0 7.07" /><path d="M19.07 4.93a10 10 0 0 1 0 14.14" />
        </svg>
        <svg v-else viewBox="0 0 24 24" width="14" height="14" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5" fill="currentColor" />
          <line x1="23" y1="9" x2="17" y2="15" /><line x1="17" y1="9" x2="23" y2="15" />
        </svg>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const props = defineProps({
  audioSrc: { type: String, required: true },
});

// Encode spaces / parentheses in the file name so the URL is always valid
const src = computed(() => encodeURI(props.audioSrc));

const audioEl = ref(null);
const trackEl = ref(null);
const isPlaying = ref(false);
const isLooping = ref(true);
const isMuted = ref(false);
const currentTime = ref(0);
const duration = ref(0);

const progress = computed(() => (duration.value ? (currentTime.value / duration.value) * 100 : 0));

const formatTime = (s) => {
  if (!s || !isFinite(s)) return '00:00';
  const m = Math.floor(s / 60);
  const sec = Math.floor(s % 60);
  return `${String(m).padStart(2, '0')}:${String(sec).padStart(2, '0')}`;
};

const onLoaded = () => { duration.value = audioEl.value.duration; };
const onTimeUpdate = () => { currentTime.value = audioEl.value.currentTime; };

// When the song finishes, start it again from the beginning (loop)
const onEnded = async () => {
  const a = audioEl.value;
  if (!a) return;
  if (!isLooping.value) { isPlaying.value = false; return; }
  a.currentTime = 0;
  try { await a.play(); } catch (_) { isPlaying.value = false; }
};

const togglePlay = async () => {
  const a = audioEl.value;
  if (!a) return;
  if (a.paused) {
    try { await a.play(); } catch (e) { console.warn('No se pudo reproducir el audio:', e); }
  } else {
    a.pause();
  }
};

const skip = (secs) => {
  const a = audioEl.value;
  if (!a) return;
  a.currentTime = Math.min(Math.max(0, a.currentTime + secs), duration.value || 0);
};

const toggleMute = () => {
  isMuted.value = !isMuted.value;
  if (audioEl.value) audioEl.value.muted = isMuted.value;
};

// Floating button: if paused, play with sound; otherwise toggle mute
const onFloatingClick = async () => {
  const a = audioEl.value;
  if (!a) return;
  if (a.paused) {
    isMuted.value = false;
    a.muted = false;
    try {
      await a.play();
    } catch (e) {
      console.warn('No se pudo reproducir el audio:', e);
    }
  } else {
    toggleMute();
  }
};

const isMounted = ref(false);

onMounted(() => {
  isMounted.value = true;
  if (audioEl.value) {
    audioEl.value.volume = 1;
  }
});

const seek = (e) => {
  const a = audioEl.value;
  if (!a || !duration.value) return;
  const rect = trackEl.value.getBoundingClientRect();
  const ratio = Math.min(Math.max(0, (e.clientX - rect.left) / rect.width), 1);
  a.currentTime = ratio * duration.value;
};
</script>

<style scoped>
.music-player {
  position: relative;
  z-index: 1;
  width: 88%;
  margin: 2.25rem auto 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.55rem;
  user-select: none;
  color: var(--color-brown, #633917);
}

.player-hint {
  font-family: var(--font-sans);
  font-size: 0.62rem;
  letter-spacing: 0.2em;
  color: var(--color-brown, #633917);
  opacity: 0.85;
  margin: 0;
}

.progress-row {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  width: 100%;
}

.time {
  font-family: var(--font-sans);
  font-size: 0.58rem;
  letter-spacing: 0.05em;
  min-width: 2.4rem;
  text-align: center;
  opacity: 0.8;
}

.progress-track {
  position: relative;
  flex: 1;
  height: 3px;
  background: rgba(99, 57, 23, 0.25);
  border-radius: 2px;
  cursor: pointer;
}

/* Bigger invisible hit area for touch */
.progress-track::before {
  content: '';
  position: absolute;
  inset: -8px 0;
}

.progress-fill {
  position: absolute;
  inset: 0 auto 0 0;
  background: var(--color-brown, #633917);
  border-radius: 2px;
}

.progress-thumb {
  position: absolute;
  top: 50%;
  width: 9px;
  height: 9px;
  background: var(--color-brown, #633917);
  border-radius: 50%;
  transform: translate(-50%, -50%);
}

.controls {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.4rem;
}

.ctrl {
  background: none;
  border: none;
  padding: 0.25rem;
  color: var(--color-brown, #633917);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.15s ease, opacity 0.15s ease, color 0.15s ease;
}

.ctrl:hover { transform: scale(1.12); }
.ctrl:active { transform: scale(0.95); }

.ctrl.small { opacity: 0.55; }
.ctrl.small.active { opacity: 1; color: #648550; }

.ctrl.play { color: var(--color-brown, #633917); }

/* ---------- FLOATING MUTE BUTTON (fixed on the side) ---------- */
.floating-mute-btn {
  position: fixed;
  right: 16px;
  bottom: 24px;
  z-index: 9999;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  border: 1.5px solid rgba(127, 96, 0, 0.55);
  background: #648550;
  color: #FFFFFF;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 6px 18px rgba(25, 38, 18, 0.35);
  transition: transform 0.2s ease, background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.floating-mute-btn:hover { transform: scale(1.08); }
.floating-mute-btn:active { transform: scale(0.94); }

.floating-mute-btn.muted {
  background: #FBF7EF;
  color: var(--color-brown, #633917);
}

.floating-mute-btn svg { position: relative; z-index: 1; }

.pulse-ring {
  position: absolute;
  inset: 0;
  border-radius: 50%;
  border: 2px solid #648550;
  animation: pulse 1.8s ease-out infinite;
}

@keyframes pulse {
  0% { transform: scale(1); opacity: 0.8; }
  100% { transform: scale(1.6); opacity: 0; }
}

@media (prefers-reduced-motion: reduce) {
  .pulse-ring { animation: none; }
}
</style>
