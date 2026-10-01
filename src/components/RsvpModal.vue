<template>
  <Teleport to="body">
    <Transition name="fade">
      <div v-if="isOpen" class="modal-backdrop" @click.self="closeModal">
        <div class="modal-card">
          <button class="close-btn" @click="closeModal" aria-label="Cerrar modal">&times;</button>
          
          <div class="modal-header">
            <span class="modal-subtitle">Boda Katherine & Enrry</span>
            <h2 class="modal-title">Confirmar Asistencia</h2>
            <p class="planner-subtitle">
              Wedding Planner: <strong>Amparo Ortiz</strong> (+51 991 399 575)
            </p>
            <div class="decorative-line"><span>✦</span></div>
          </div>

          <div v-if="submitted" class="success-message">
            <div class="success-icon">✓</div>
            <h3>¡Gracias por confirmar!</h3>
            <p>Hemos registrado tu respuesta. Te redirigiremos al WhatsApp de nuestra Wedding Planner <strong>Amparo Ortiz</strong> (+51 991 399 575) para enviar la confirmación.</p>
            <button class="action-btn" @click="sendWhatsApp">Enviar a WhatsApp</button>
          </div>

          <form v-else @submit.prevent="handleSubmit" class="rsvp-form">
            <div class="form-group">
              <label>Nombre Completo / Familia:</label>
              <input 
                type="text" 
                v-model="form.name" 
                placeholder="Ej. Familia López / Juan Pérez" 
                required 
              />
            </div>

            <div class="form-group">
              <label>¿Nos acompañarás?</label>
              <div class="radio-options">
                <label class="radio-card" :class="{ active: form.attending === 'yes' }">
                  <input type="radio" v-model="form.attending" value="yes" />
                  <span>¡Sí, asistiré con gusto! 🎉</span>
                </label>
                <label class="radio-card" :class="{ active: form.attending === 'no' }">
                  <input type="radio" v-model="form.attending" value="no" />
                  <span>Lo siento, no podré asistir 🤍</span>
                </label>
              </div>
            </div>

            <div v-if="form.attending === 'yes'" class="form-row">
              <div class="form-group half">
                <label>Adultos:</label>
                <select v-model.number="form.adults">
                  <option v-for="n in 10" :key="n" :value="n">{{ n }}</option>
                </select>
              </div>
              <div class="form-group half">
                <label>Niños:</label>
                <select v-model.number="form.children">
                  <option v-for="n in 11" :key="n-1" :value="n-1">{{ n-1 }}</option>
                </select>
              </div>
            </div>

            <div class="form-group">
              <label>Mensaje o consultas para Amparo Ortiz (opcional):</label>
              <textarea 
                v-model="form.notes" 
                rows="2" 
                placeholder="Escribe alguna consulta sobre el evento o mensaje para los novios..."
              ></textarea>
            </div>

            <button type="submit" class="submit-btn">
              Confirmar por WhatsApp con Amparo Ortiz
            </button>
          </form>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { ref, reactive, watch } from 'vue';

const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false
  },
  guestName: {
    type: String,
    default: ''
  },
  phone: {
    type: String,
    default: '51991399575'
  }
});

const emit = defineEmits(['update:modelValue']);
const isOpen = ref(props.modelValue);
const submitted = ref(false);

const form = reactive({
  name: props.guestName || '',
  attending: 'yes',
  adults: 2,
  children: 0,
  notes: ''
});

watch(() => props.modelValue, (val) => {
  isOpen.value = val;
  if (val) submitted.value = false;
});

watch(() => props.guestName, (val) => {
  if (val) form.name = val;
});

function closeModal() {
  isOpen.value = false;
  emit('update:modelValue', false);
}

function handleSubmit() {
  submitted.value = true;
  sendWhatsApp();
}

function sendWhatsApp() {
  const statusText = form.attending === 'yes' ? '¡Sí, asistiré!' : 'Lamentablemente no podré asistir.';
  let text = `¡Hola Amparo Ortiz! ✨\nConfirmo mi asistencia a la Boda de Katherine y Enrry (21 Nov 2026).\n\n` +
             `*Nombre:* ${form.name}\n` +
             `*Respuesta:* ${statusText}\n`;
             
  if (form.attending === 'yes') {
    text += `*Nº de Adultos:* ${form.adults}\n` +
            `*Nº de Niños:* ${form.children}\n`;
  }
  
  if (form.notes) {
    text += `*Nota / Consulta:* ${form.notes}\n`;
  }

  const encodedText = encodeURIComponent(text);
  const targetPhone = props.phone ? props.phone.replace(/[^0-9]/g, '') : '51991399575';
  const url = `https://wa.me/${targetPhone}?text=${encodedText}`;
  window.open(url, '_blank');
}
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  inset: 0;
  z-index: 1000;
  background: rgba(45, 35, 31, 0.75);
  backdrop-filter: blur(6px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.25rem;
}

.modal-card {
  position: relative;
  width: 100%;
  max-width: 440px;
  background: #F4EEDF;
  background-image: radial-gradient(circle at 50% 0%, rgba(255,255,255,0.7) 0%, transparent 80%);
  border: 1.5px solid var(--color-gold, #7F6000);
  border-radius: 16px;
  padding: 2rem 1.5rem;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
  max-height: 90vh;
  overflow-y: auto;
}

.close-btn {
  position: absolute;
  top: 12px;
  right: 16px;
  background: none;
  border: none;
  font-size: 1.8rem;
  color: var(--color-brown);
  cursor: pointer;
}

.modal-header {
  text-align: center;
  margin-bottom: 1.5rem;
}

.modal-subtitle {
  font-family: var(--font-sans);
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  color: var(--color-brown);
  display: block;
  margin-bottom: 0.25rem;
}

.modal-title {
  font-family: var(--font-script);
  font-size: 2.2rem;
  color: var(--color-brown);
  font-weight: normal;
}

.planner-subtitle {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  color: var(--color-brown, #633917);
  margin-top: 0.3rem;
}

.decorative-line {
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-gold);
  font-size: 0.8rem;
  margin-top: 0.25rem;
}

.rsvp-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.form-group label {
  font-family: var(--font-sans);
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--color-brown);
}

.form-group input[type="text"],
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid rgba(127, 96, 0, 0.35);
  border-radius: 8px;
  background: #FFFFFF;
  font-family: var(--font-sans);
  font-size: 0.9rem;
  color: var(--color-brown);
  outline: none;
  transition: border-color 0.2s;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  border-color: var(--color-gold);
}

.radio-options {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.radio-card {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem;
  border: 1px solid rgba(99, 57, 23, 0.2);
  border-radius: 8px;
  background: #FFFFFF;
  cursor: pointer;
  font-family: var(--font-sans);
  font-size: 0.85rem;
  transition: all 0.2s;
}

.radio-card.active {
  border-color: var(--color-gold);
  background: rgba(127, 96, 0, 0.08);
}

.form-row {
  display: flex;
  gap: 1rem;
}

.form-group.half {
  flex: 1;
}

.submit-btn, .action-btn {
  width: 100%;
  padding: 0.9rem;
  border: none;
  border-radius: 8px;
  background-color: var(--color-brown, #633917);
  color: #FFFFFF;
  font-family: var(--font-sans);
  font-weight: 700;
  font-size: 0.85rem;
  letter-spacing: 0.05em;
  text-transform: uppercase;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.1s;
  box-shadow: 0 4px 12px rgba(99, 57, 23, 0.3);
  border: 1.5px solid var(--color-gold, #7F6000);
  margin-top: 0.5rem;
}

.submit-btn:hover, .action-btn:hover {
  background-color: var(--color-gold, #7F6000);
  color: #FFFFFF;
}

.success-message {
  text-align: center;
  padding: 1rem 0;
}

.success-icon {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: rgba(127, 96, 0, 0.15);
  color: var(--color-brown);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  font-weight: bold;
  margin: 0 auto 1rem;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
