<template>
  <div 
    class="image-placeholder-container" 
    :class="[shapeClass, tornEffect ? 'torn-edge' : '', borderStyle]"
    :style="{ aspectRatio: aspectRatio || '4/3' }"
    @click="handleClick"
  >
    <img 
      v-if="src" 
      :src="src" 
      :alt="alt || label || 'Foto de la boda'" 
      class="placeholder-img"
      @error="handleImgError"
    />
    
    <div v-else class="placeholder-fallback">
      <div class="placeholder-icon">
        <svg xmlns="http://www.w3.org/2000/svg" width="36" height="36" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
          <rect width="18" height="18" x="3" y="3" rx="2" ry="2"/>
          <circle cx="9" cy="9" r="2"/>
          <path d="m21 15-3.086-3.086a2 2 0 0 0-2.828 0L6 21"/>
        </svg>
      </div>
      <span class="placeholder-label">{{ label || 'Espacio para imagen' }}</span>
      <span class="placeholder-sublabel">{{ sublabel || 'Haz clic para cargar o pon tu imagen en /public/images/' }}</span>
    </div>

    <!-- Hidden file input so user can test uploading an image directly in preview -->
    <input 
      type="file" 
      ref="fileInput" 
      accept="image/*" 
      class="hidden-file-input" 
      @change="handleFileChange"
    />
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
  initialSrc: {
    type: String,
    default: ''
  },
  label: {
    type: String,
    default: 'Espacio para imagen'
  },
  sublabel: {
    type: String,
    default: 'Reemplaza con tu foto'
  },
  alt: {
    type: String,
    default: ''
  },
  aspectRatio: {
    type: String,
    default: '4/3'
  },
  shape: {
    type: String,
    default: 'rect' // 'rect', 'arch', 'circle'
  },
  tornEffect: {
    type: Boolean,
    default: false
  },
  borderStyle: {
    type: String,
    default: 'gold' // 'gold', 'sage', 'none'
  }
});

const emit = defineEmits(['update:image']);
const src = ref(props.initialSrc);
const fileInput = ref(null);

const shapeClass = ref(`shape-${props.shape}`);

watch(() => props.initialSrc, (newVal) => {
  src.value = newVal;
});

function handleImgError() {
  // If image fails to load (e.g. file doesn't exist yet in public/images/), fallback to placeholder
  src.value = '';
}

function handleClick() {
  if (fileInput.value) {
    fileInput.value.click();
  }
}

function handleFileChange(event) {
  const file = event.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = (e) => {
      src.value = e.target.result;
      emit('update:image', e.target.result);
    };
    reader.readAsDataURL(file);
  }
}
</script>

<style scoped>
.image-placeholder-container {
  position: relative;
  width: 100%;
  background-color: var(--color-bg-alt, #F4EFEA);
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(99, 57, 23, 0.08);
}

.image-placeholder-container:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(99, 57, 23, 0.12);
}

.shape-rect {
  border-radius: 8px;
}

.shape-arch {
  border-top-left-radius: 120px;
  border-top-right-radius: 120px;
  border-bottom-left-radius: 8px;
  border-bottom-right-radius: 8px;
}

.shape-circle {
  border-radius: 50%;
}

.gold {
  border: 1px solid rgba(127, 96, 0, 0.3);
}

.sage {
  border: 1px solid rgba(100, 133, 80, 0.4);
}

.placeholder-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.placeholder-fallback {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1.5rem;
  text-align: center;
  color: var(--color-brown, #633917);
}

.placeholder-icon {
  color: var(--color-sage, #648550);
  margin-bottom: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.placeholder-label {
  font-family: var(--font-serif);
  font-weight: 600;
  font-size: 0.95rem;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  color: var(--color-brown);
  margin-bottom: 0.25rem;
}

.placeholder-sublabel {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  color: var(--color-text-muted, #8C7565);
  font-style: italic;
}

/* Torn paper edge simulation */
.torn-edge {
  clip-path: polygon(
    0% 2%, 5% 0%, 10% 2%, 15% 0%, 20% 1%, 25% 0%, 30% 2%, 35% 0%, 40% 1%, 45% 0%, 50% 2%, 55% 0%, 60% 1%, 65% 0%, 70% 2%, 75% 0%, 80% 1%, 85% 0%, 90% 2%, 95% 0%, 100% 2%,
    100% 98%, 95% 100%, 90% 98%, 85% 100%, 80% 99%, 75% 100%, 70% 98%, 65% 100%, 60% 99%, 55% 100%, 50% 98%, 45% 100%, 40% 99%, 35% 100%, 30% 98%, 25% 100%, 20% 99%, 15% 100%, 10% 98%, 5% 100%, 0% 98%
  );
}

.hidden-file-input {
  display: none;
}
</style>
