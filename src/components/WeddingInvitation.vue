<template>
  <div class="invitation-wrapper">
    <!-- DEFINICIÓN DEL FILTRO SVG: TEXTURA DE PAPEL ALGODÓN/ARTESANAL CON DOS CAPAS -->
    <svg width="0" height="0" class="svg-defs" aria-hidden="true">
      <defs>
        <!-- Filtro Procedural de Papel Artesanal (Relieve + Fibras Finas) -->
        <filter id="papel-algodon" x="0%" y="0%" width="100%" height="100%" color-interpolation-filters="sRGB">
          <!-- Capa 1: Relieve y porosidad base del papel -->
          <feTurbulence type="fractalNoise" baseFrequency="0.035" numOctaves="5" seed="7" result="granoBase"/>
          <feDiffuseLighting in="granoBase" lighting-color="#FBF7EF" surfaceScale="1.6" result="luzRelieve">
            <feDistantLight azimuth="45" elevation="70"/>
          </feDiffuseLighting>
          
          <!-- Capa 2: Fibras finas y micro-rugosidad del algodón -->
          <feTurbulence type="fractalNoise" baseFrequency="0.4" numOctaves="3" seed="15" result="fibrasFinas"/>
          <feColorMatrix type="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 0.05 0" in="fibrasFinas" result="fibrasAlpha"/>
          
          <!-- Mezcla final de la luz del relieve con las fibras finas -->
          <feBlend mode="multiply" in="fibrasAlpha" in2="luzRelieve" result="texturaFinal"/>
        </filter>
      </defs>
    </svg>



    <!-- Contenedor Principal de la Invitación Móvil -->
    <main class="mobile-card-container">
      
      <!-- CAPA DE FONDO TEXTURIZADO CON FILTRO SVG -->
      <div class="fondo-papel-capa" aria-hidden="true">
        <svg class="fondo-papel-svg" width="100%" height="100%">
          <rect width="100%" height="100%" filter="url(#papel-algodon)"/>
        </svg>
      </div>

      <!-- FLOATING / STICKY AUDIO CONTROLLER (Hidden) -->
      <!-- <AudioPlayer :audioSrc="audioPath" /> -->

      <!-- COVER / HERO SECTION 1 -->
      <section class="card-section hero-section">
        <!-- Top Monogram & Names Header -->
        <div class="top-header">
          <h1 class="couple-names">ENRRY & KATHERINE</h1>
          <div class="header-divider">
            <span class="line"></span>
            <span class="diamond">❖</span>
            <span class="line"></span>
          </div>
        </div>

        <!-- Hero Dual Photo Placeholders (Main Vertical + Secondary Arch) -->
        <div class="hero-photo-composition">
          <!-- Main Vertical Couple Photo Space -->
          <div class="main-photo-wrapper">
            <ImagePlaceholder 
              :initialSrc="heroPhotoMain"
              label="Foto Principal Novios"
              sublabel="Enrry & Katherine"
              aspectRatio="3/4"
              shape="rect"
              borderStyle="gold"
              @update:image="heroPhotoMain = $event"
            />
          </div>

          <!-- Overlapping Secondary Arch Photo Space -->
          <div class="secondary-arch-photo">
            <ImagePlaceholder 
              :initialSrc="heroPhotoOverlay"
              label="Foto Detalle"
              sublabel="Amor & Alegría"
              aspectRatio="1/1"
              shape="circle"
              borderStyle="rose"
              @update:image="heroPhotoOverlay = $event"
            />
          </div>
        </div>

        <!-- Music Player -->
        <AudioPlayer :audioSrc="audioPath" />

        <!-- Romantic Quote / Declaration -->
        <div class="romantic-quote-box">
          <p class="quote-text">
            El Señor, hizo coincidir nuestros caminos en Cusco y nos regaló una bonita amistad. Sin buscarlo, esa amistad fue creciendo y se convirtió en amor. Desde entonces hemos aprendido a caminar juntos, compartir sueños, desafíos y descubrir un propósito común. Hoy volvemos al lugar donde comenzó nuestra historia para iniciar una nueva etapa, construyendo un hogar con Cristo en el centro.
          </p>
          <p class="quote-verse">
            “Si el SEÑOR no edifica la casa, en vano trabajan los que la edifican; si el SEÑOR no guarda la ciudad, en vano vela la guardia” Salmo 127:1
          </p>
        </div>
      </section>

      <!-- SECTION 2: NUESTROS PADRES -->
      <section class="card-section parents-section">
        <h2 class="script-title">Nuestros padres</h2>
        
        <div class="parents-grid">
          <!-- Parents of the Bride (From instructions.md) -->
          <div class="parent-card bride-parents">
            <span class="parent-role bride-role">PADRES DE LA NOVIA</span>
            <p class="parent-name">Francisco Antonio Gutierrez Condori</p>
            <p class="parent-name">María Mercedes Otero Aymara</p>
          </div>

          <div class="section-ornament">
            <svg width="60" height="24" viewBox="0 0 60 24" fill="none">
              <path d="M5 12 C 20 2, 40 22, 55 12" stroke="#E28E8C" stroke-width="1.5" fill="none"/>
              <circle cx="30" cy="12" r="4" fill="#F0C7C5" stroke="#E28E8C" stroke-width="1"/>
            </svg>
          </div>

          <!-- Parents of the Groom -->
          <div class="parent-card groom-parents">
            <span class="parent-role">PADRES DEL NOVIO</span>
            <p class="parent-name">Enry López Ruiz</p>
            <p class="parent-name">Ana Cecilia del Aguila Pinedo</p>
          </div>
        </div>
      </section>

      <!-- SECTION 3: NUESTROS PADRINOS -->
      <section class="card-section godparents-section">
        <h2 class="script-title">Nuestros padrinos</h2>

        <div class="godparents-card">
          <span class="godparent-role">PADRINOS DE BODA</span>
          <p class="godparent-name">Carlos Fasanando Mendieta</p>
          <div class="godparent-divider">
            <span class="divider-line"></span>
            <span class="divider-heart">✦</span>
            <span class="divider-line"></span>
          </div>
          <p class="godparent-name">Becky Grandez Dávalos</p>
        </div>
      </section>

      <!-- SECTION 4: GUARDA LA FECHA (SAVE THE DATE) -->
      <section class="card-section save-date-section">
        <h2 class="script-title">Guarda la fecha</h2>

        <!-- Date Banner -->
        <div class="date-banner">
          <div class="date-side left-side">
            <span class="date-day-name">SÁBADO</span>
            <span class="date-divider">•</span>
          </div>
          <span class="date-day-num">21</span>
          <div class="date-side right-side">
            <span class="date-divider">•</span>
            <span class="date-month-name">NOVIEMBRE</span>
          </div>
        </div>
        <div class="date-year">2026</div>

        <!-- Countdown Timer -->
        <CountdownTimer />

        <!-- Full Portrait Image Space -->
        <div class="full-portrait-space">
          <ImagePlaceholder 
            :initialSrc="saveDatePhoto"
            label="Foto Guarda la Fecha"
            sublabel="Enrry & Katherine - 2026"
            aspectRatio="3/4"
            shape="rect"
            tornEffect
            borderStyle="rose"
            @update:image="saveDatePhoto = $event"
          />
        </div>
      </section>

      <!-- SECTION 4: CEREMONIA & RECEPCIÓN -->
      <section class="card-section reception-section">
        <h2 class="script-title">Ceremonia</h2>

        <!-- Clinking Glasses Icon -->
        <div class="section-icon">
          <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="#633917" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M8 22h8"/>
            <path d="M12 15v7"/>
            <path d="M12 15a5 5 0 0 0 5-5V3H7v7a5 5 0 0 0 5 5Z"/>
            <path d="m15 3 2 4"/>
          </svg>
        </div>

        <div class="venue-info">
          <div class="schedule-box">
            <div class="schedule-item">
              <span class="time-tag">11:00 A.M.</span>
              <span class="event-label">RECEPCIÓN</span>
            </div>
            <div class="schedule-divider"></div>
            <div class="schedule-item">
              <span class="time-tag">12:00 P.M.</span>
              <span class="event-label">CEREMONIA</span>
            </div>
          </div>
          
          <h3 class="venue-name">Llaullipata La Casona de la Princesita</h3>
          <p class="venue-address">Llaullipata, Cusco, Perú</p>
        </div>

        <!-- Venue Photo Space -->
        <div class="venue-photo-container">
          <ImagePlaceholder 
            :initialSrc="venuePhoto"
            label="Foto de la Casona / Lugar"
            sublabel="Llaullipata La Casona de la Princesita"
            aspectRatio="16/9"
            shape="rect"
            borderStyle="gold"
            @update:image="venuePhoto = $event"
          />
        </div>

        <!-- Google Maps Link Button -->
        <a 
          :href="mapsUrl" 
          target="_blank" 
          rel="noopener noreferrer" 
          class="maps-btn"
        >
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"/>
            <circle cx="12" cy="10" r="3"/>
          </svg>
          Ver Ubicación en Google Maps
        </a>
      </section>

      <!-- SECTION 5: DRESS CODE -->
      <section class="card-section dresscode-section">
        <h2 class="script-title">Dress Code</h2>

        <!-- Suit & Dress Silhouettes -->
        <div class="dresscode-icons">
          <div class="dresscode-icon-box">
            <svg width="64" height="64" viewBox="0 0 64 64" fill="none" stroke="#633917" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <path d="M18 12 L24 24 L24 52 L12 52 L12 24 Z" fill="rgba(99, 57, 23, 0.08)" />
              <path d="M18 12 L18 28" />
              <polygon points="18,16 16,14 20,14" fill="#7F6000" />
              <path d="M46 12 C42 12, 38 18, 40 26 L34 52 L54 52 L48 26 C50 18, 46 12, 46 12 Z" fill="rgba(240, 199, 197, 0.4)" stroke="#E28E8C" stroke-width="1.5" />
            </svg>
          </div>
        </div>

        <h3 class="dresscode-type">Código de Vestimenta: Formal</h3>

        <p class="dresscode-note">
          Por favor evita el color blanco, ese color queda reservado exclusivamente para la novia.
        </p>
      </section>

      <!-- SECTION 6: FLORAL MONOGRAM FRAME -->
      <section class="card-section monogram-section">
        <div class="monogram-roses-container">
          <img :src="rosesTopImg" alt="Guirnalda Rosas Superior" class="roses-top-img" />
          <div class="monogram-text">E & K</div>
        </div>
        <p class="monogram-verse">
          “Por mi parte, mi casa y yo serviremos al Señor.” Josué 24:15
        </p>
      </section>

      <!-- SECTION 7: ITINERARIO DE BODA (TIMELINE) -->
      <section class="card-section timeline-section">
        <h2 class="script-title">Itinerario de Boda</h2>

        <div class="timeline-container">
          <div class="timeline-line"></div>

          <!-- Timeline Item 1 -->
          <div class="timeline-item left">
            <div class="timeline-content">
              <span class="timeline-time">11:00 A.M.</span>
              <span class="timeline-title">RECEPCIÓN</span>
            </div>
            <div class="timeline-node rose-node">
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M8 22h8"/><path d="M12 15v7"/><path d="M12 15a5 5 0 0 0 5-5V3H7v7a5 5 0 0 0 5 5Z"/></svg>
            </div>
          </div>

          <!-- Timeline Item 2 -->
          <div class="timeline-item right">
            <div class="timeline-node gold-node">
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="9" cy="12" r="6"/><circle cx="15" cy="12" r="6"/></svg>
            </div>
            <div class="timeline-content">
              <span class="timeline-time">12:00 P.M.</span>
              <span class="timeline-title">CEREMONIA CIVIL</span>
            </div>
          </div>

          <!-- Timeline Item 3 -->
          <div class="timeline-item left">
            <div class="timeline-content">
              <span class="timeline-time">02:00 P.M.</span>
              <span class="timeline-title">ALMUERZO</span>
            </div>
            <div class="timeline-node rose-node">
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 2v7c0 1.1.9 2 2 2h4a2 2 0 0 0 2-2V2"/><path d="M7 2v20"/><path d="M21 15V2v0a5 5 0 0 0-5 5v6c0 1.1.9 2 2 2h3Zm0 0v7"/></svg>
            </div>
          </div>

          <!-- Timeline Item 4 -->
          <div class="timeline-item right">
            <div class="timeline-node gold-node">
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 18V5l12-2v13"/><circle cx="6" cy="18" r="3"/><circle cx="18" cy="16" r="3"/></svg>
            </div>
            <div class="timeline-content">
              <span class="timeline-time">03:30 P.M.</span>
              <span class="timeline-title">INICIO DE PROTOCOLO</span>
            </div>
          </div>

          <!-- Timeline Item 5 -->
          <div class="timeline-item left">
            <div class="timeline-content">
              <span class="timeline-time">07:00 P.M.</span>
              <span class="timeline-title">NOS DESPEDIMOS</span>
            </div>
            <div class="timeline-node rose-node">
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M19 17h2c.6 0 1-.4 1-1v-3c0-.9-.7-1.7-1.5-1.9C18.7 10.6 16 10 16 10s-1.3-1.4-2.2-2.3c-.5-.4-1.1-.7-1.8-.7H5c-.6 0-1.1.4-1.4.9l-1.5 2.8C2.05 11.2 2 11.6 2 12v4c0 .6.4 1 1 1h2"/><circle cx="7" cy="17" r="2"/><circle cx="17" cy="17" r="2"/></svg>
            </div>
          </div>
        </div>

        <!-- Bottom Roses Arch -->
        <div class="bottom-wreath">
          <img :src="rosesBottomImg" alt="Guirnalda Rosas Inferior" class="roses-bottom-img" />
        </div>
      </section>

      <!-- SECTION 8: GALLERY PHOTO SLIDER / GRID -->
      <section class="card-section gallery-section">
        <h2 class="script-title">Nuestra Galería</h2>

        <div class="gallery-photo-frame">
          <transition name="slide-fade" mode="out-in">
            <div :key="galleryPhoto" class="gallery-slide-wrapper">
              <ImagePlaceholder 
                :initialSrc="galleryPhoto"
                label="Foto Galería Novios"
                sublabel="Enrry & Katherine"
                aspectRatio="3/4"
                shape="rect"
                borderStyle="rose"
                @update:image="galleryPhoto = $event"
              />
            </div>
          </transition>
        </div>

        <!-- Additional Photo Grid Carousel -->
        <div class="mini-gallery-grid">
          <div 
            v-for="(imgSrc, idx) in allGalleryImages" 
            :key="idx" 
            class="mini-gallery-item"
            :class="{ 'is-active': galleryPhoto === imgSrc }"
            @click="selectGalleryPhoto(imgSrc, idx)"
          >
            <img :src="imgSrc" alt="Momento Enrry y Katherine" />
          </div>
        </div>
      </section>

      <!-- GUEST PASS CARD (Pases de Invitados) -->
      <div class="guest-pass-card">
        <div class="pass-header">
          <span class="pass-title">PASE DE INVITACIÓN ESPECIAL</span>
        </div>
        <div class="pass-body">
          <h3 class="guest-family">{{ guestName }}</h3>
          <div class="pass-details">
            <div class="pass-badge brown-badge">
              <span class="badge-label">Adultos:</span>
              <span class="badge-value">{{ adultPasses }}</span>
            </div>
            <div class="pass-badge rose-badge">
              <span class="badge-label">Niños:</span>
              <span class="badge-value">{{ childPasses }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- SECTION 9: REGALOS (GIFTS) -->
      <section class="card-section gifts-section">
        <h2 class="script-title">Regalos</h2>

        <div class="section-icon gift-icon-wrap">
          <svg xmlns="http://www.w3.org/2000/svg" width="42" height="42" viewBox="0 0 24 24" fill="none" stroke="#633917" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <rect x="3" y="8" width="18" height="4" rx="1"/>
            <path d="M12 8v13"/>
            <path d="M19 12v7a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2v-7"/>
            <path d="M7.5 8a2.5 2.5 0 0 1 0-5A4.8 4.8 0 0 1 12 8a4.8 4.8 0 0 1 4.5-5 2.5 2.5 0 0 1 0 5"/>
          </svg>
        </div>

        <p class="gifts-phrase">
          “Tu presencia es nuestro mejor regalo. Si quieres acompañarnos con algo más, sea un detalle, una experiencia o un aporte en efectivo, lo recibiremos con todo el cariño del mundo.”
        </p>

        <div class="gift-accounts-container">
          <!-- Yape Card -->
          <div class="account-card">
            <div class="account-header">
              <div class="account-badge yape-badge">
                <span>YAPE</span>
              </div>
              <span class="account-owner">Enrry & Katherine</span>
            </div>
            <div class="account-row">
              <div class="account-info">
                <span class="account-label">Número de celular</span>
                <span class="account-number">984 851 184</span>
              </div>
              <button 
                class="copy-btn" 
                :class="{ 'is-copied': copiedField === 'yape' }"
                @click="copyToClipboard('984851184', 'yape')"
                aria-label="Copiar número de Yape"
              >
                <span v-if="copiedField === 'yape'" class="copy-feedback">
                  <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                    <polyline points="20 6 9 17 4 12"/>
                  </svg>
                  Copiado
                </span>
                <span v-else class="copy-action">
                  <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect width="14" height="14" x="8" y="8" rx="2" ry="2"/>
                    <path d="M4 16c-1.1 0-2-.9-2-2V4c0-1.1.9-2 2-2h10c1.1 0 2 .9 2 2"/>
                  </svg>
                  Copiar
                </span>
              </button>
            </div>
          </div>

          <!-- Interbank Card -->
          <div class="account-card">
            <div class="account-header">
              <div class="account-badge bank-badge">
                <span>INTERBANK</span>
              </div>
              <span class="account-owner">Cuenta Soles</span>
            </div>

            <!-- Cuenta Soles -->
            <div class="account-row">
              <div class="account-info">
                <span class="account-label">Número de cuenta</span>
                <span class="account-number">3403208431701</span>
              </div>
              <button 
                class="copy-btn" 
                :class="{ 'is-copied': copiedField === 'cuenta' }"
                @click="copyToClipboard('3403208431701', 'cuenta')"
                aria-label="Copiar número de cuenta Interbank"
              >
                <span v-if="copiedField === 'cuenta'" class="copy-feedback">
                  <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                    <polyline points="20 6 9 17 4 12"/>
                  </svg>
                  Copiado
                </span>
                <span v-else class="copy-action">
                  <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect width="14" height="14" x="8" y="8" rx="2" ry="2"/>
                    <path d="M4 16c-1.1 0-2-.9-2-2V4c0-1.1.9-2 2-2h10c1.1 0 2 .9 2 2"/>
                  </svg>
                  Copiar
                </span>
              </button>
            </div>

            <div class="account-divider"></div>

            <!-- CCI -->
            <div class="account-row">
              <div class="account-info">
                <span class="account-label">CCI (Interbancario)</span>
                <span class="account-number cci-number">00334001320843170176</span>
              </div>
              <button 
                class="copy-btn" 
                :class="{ 'is-copied': copiedField === 'cci' }"
                @click="copyToClipboard('00334001320843170176', 'cci')"
                aria-label="Copiar CCI Interbank"
              >
                <span v-if="copiedField === 'cci'" class="copy-feedback">
                  <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                    <polyline points="20 6 9 17 4 12"/>
                  </svg>
                  Copiado
                </span>
                <span v-else class="copy-action">
                  <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <rect width="14" height="14" x="8" y="8" rx="2" ry="2"/>
                    <path d="M4 16c-1.1 0-2-.9-2-2V4c0-1.1.9-2 2-2h10c1.1 0 2 .9 2 2"/>
                  </svg>
                  Copiar
                </span>
              </button>
            </div>
          </div>
        </div>
      </section>

      <!-- SECTION 10: CONFIRMAR ASISTENCIA (RSVP) -->
      <section class="card-section rsvp-section">
        <h2 class="script-title">Confirmar Asistencia</h2>

        <p class="rsvp-text">
          NUESTRO DÍA SERÁ AÚN MÁS ESPECIAL SI ESTÁS CON NOSOTROS. CONFIRMA TU ASISTENCIA ANTES DEL 01 DE NOVIEMBRE DE 2026.
        </p>

        <!-- Wedding Planner Amparo Ortiz Info Box -->
        <div class="planner-info-box">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#9E4B49" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"/>
          </svg>
          <span>Confirmar asistencia o alguna duda con nuestra Wedding Planner <strong>Amparo Ortiz</strong></span>
        </div>

        <!-- Main RSVP Button -->
        <button class="rsvp-main-btn" @click="isRsvpModalOpen = true">
          Confirmar Asistencia
        </button>
      </section>

      <!-- FOOTER CREDITS -->
      <footer class="invitation-footer">
        <div class="footer-gold-line"></div>
        <p class="footer-blessing">Enrry Alberto & Katherine Francis</p>
        <p class="footer-date">21 • 11 • 2026</p>
      </footer>
    </main>

    <!-- INTERACTIVE RSVP MODAL COMPONENT -->
    <RsvpModal 
      v-model="isRsvpModalOpen" 
      :guestName="guestName"
      phone="51991399575"
    />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import ImagePlaceholder from './ImagePlaceholder.vue';
import AudioPlayer from './AudioPlayer.vue';
import RsvpModal from './RsvpModal.vue';
import CountdownTimer from './CountdownTimer.vue';

const base = import.meta.env.BASE_URL.endsWith('/') ? import.meta.env.BASE_URL : import.meta.env.BASE_URL + '/';

const heroPhotoMain = ref(`${base}images/opt__DSC6406.jpg`);
const heroPhotoOverlay = ref(`${base}images/opt__DSC5761.jpg`);
const saveDatePhoto = ref(`${base}images/opt__DSC6015.jpg`);
const venuePhoto = ref(`${base}images/opt__DSC6115.jpg`);
const rosesTopImg = ref(`${base}images/roses_top.png`);
const rosesBottomImg = ref(`${base}images/roses_bottom.png`);
const audioPath = ref(`${base}audio/Arbor North - You, Me, and Jesus (Official Lyric Video).mp3`);



const allGalleryImages = [
  `${base}images/opt__DSC5516.jpg`,
  `${base}images/opt__DSC5121.jpg`,
  `${base}images/opt__DSC5525.jpg`,
  `${base}images/opt__DSC6325.jpg`
];

const galleryIndex = ref(0);
const galleryPhoto = ref(allGalleryImages[0]);
let galleryTimer = null;

const startGalleryTimer = () => {
  stopGalleryTimer();
  galleryTimer = setInterval(() => {
    galleryIndex.value = (galleryIndex.value + 1) % allGalleryImages.length;
    galleryPhoto.value = allGalleryImages[galleryIndex.value];
  }, 5000);
};

const stopGalleryTimer = () => {
  if (galleryTimer) {
    clearInterval(galleryTimer);
    galleryTimer = null;
  }
};

const selectGalleryPhoto = (imgSrc, idx) => {
  galleryPhoto.value = imgSrc;
  galleryIndex.value = idx;
  startGalleryTimer();
};

const guestName = ref('Familia / Invitado Especial');
const adultPasses = ref(1);
const childPasses = ref(0);

const isRsvpModalOpen = ref(false);

const mapsUrl = 'https://maps.app.goo.gl/KA3CrvUT6CtQRopn7?g_st=awb';

const copiedField = ref('');
let copyTimeout = null;

const copyToClipboard = async (text, fieldName) => {
  try {
    if (navigator.clipboard && navigator.clipboard.writeText) {
      await navigator.clipboard.writeText(text);
    } else {
      const textarea = document.createElement('textarea');
      textarea.value = text;
      textarea.style.position = 'fixed';
      textarea.style.opacity = '0';
      document.body.appendChild(textarea);
      textarea.select();
      document.execCommand('copy');
      document.body.removeChild(textarea);
    }
    copiedField.value = fieldName;
    if (copyTimeout) clearTimeout(copyTimeout);
    copyTimeout = setTimeout(() => {
      copiedField.value = '';
    }, 2000);
  } catch (err) {
    console.error('Error al copiar:', err);
  }
};

let scrollListener = null;

onMounted(() => {
  startGalleryTimer();

  if (typeof window !== 'undefined') {
    const params = new URLSearchParams(window.location.search);
    if (params.has('invitado')) guestName.value = params.get('invitado');
    if (params.has('adultos')) adultPasses.value = parseInt(params.get('adultos')) || 2;
    if (params.has('ninos')) childPasses.value = parseInt(params.get('ninos')) || 0;

    // Scroll Reveal Effect via IntersectionObserver
    if ('IntersectionObserver' in window) {
      const observerOptions = {
        threshold: 0.12,
        rootMargin: '0px 0px -50px 0px'
      };

      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add('is-visible');
          }
        });
      }, observerOptions);

      const elementsToReveal = document.querySelectorAll('.card-section, .guest-pass-card');
      elementsToReveal.forEach((el) => {
        el.classList.add('reveal-on-scroll');
        observer.observe(el);
      });
    } else {
      document.querySelectorAll('.card-section, .guest-pass-card').forEach((el) => {
        el.classList.add('is-visible');
      });
    }

    // Parallax background texture on scroll
    const paperBg = document.querySelector('.fondo-papel-capa');
    scrollListener = () => {
      if (paperBg) {
        const scrollY = window.scrollY;
        paperBg.style.transform = `translateY(${scrollY * 0.06}px)`;
      }
    };
    window.addEventListener('scroll', scrollListener, { passive: true });
  }
});

onUnmounted(() => {
  stopGalleryTimer();
  if (copyTimeout) clearTimeout(copyTimeout);
  if (typeof window !== 'undefined' && scrollListener) {
    window.removeEventListener('scroll', scrollListener);
  }
});
</script>

<style scoped>
.svg-defs {
  position: absolute;
  width: 0;
  height: 0;
  overflow: hidden;
}

.invitation-wrapper {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 1.5rem 0.5rem;
  background-color: #557242;
  background-image: 
    radial-gradient(circle at 50% 20%, rgba(127, 96, 0, 0.22) 0%, transparent 70%),
    radial-gradient(circle at 85% 85%, rgba(240, 199, 197, 0.35) 0%, transparent 60%),
    radial-gradient(circle at 15% 90%, rgba(99, 57, 23, 0.22) 0%, transparent 50%);
}

/* REALISTIC WARM PARCHMENT PAPER CONTAINER */
.mobile-card-container {
  width: 100%;
  max-width: 440px;
  background-color: #FBF7EF;
  border-radius: 20px;
  box-shadow: 
    0 25px 65px rgba(25, 38, 18, 0.55),
    0 4px 20px rgba(0, 0, 0, 0.25),
    inset 0 0 60px rgba(127, 96, 0, 0.08),
    inset 0 0 20px rgba(99, 57, 23, 0.05);
  border: 1.5px solid rgba(127, 96, 0, 0.45);
  overflow: hidden;
  position: relative;
  padding: 2.25rem 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 2.75rem;
}

.fondo-papel-capa {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  border-radius: inherit;
  overflow: hidden;
  will-change: transform;
}

.fondo-papel-svg {
  width: 100%;
  height: 100%;
  display: block;
}

.card-section,
.top-header,
.hero-photo-composition,
.romantic-quote-box,
.guest-pass-card,
.parents-grid,
.date-banner,
.venue-info,
.timeline-container,
.gifts-section,
.godparents-section,
.rsvp-section,
.invitation-footer {
  position: relative;
  z-index: 1;
}

.card-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  width: 100%;
}

/* SECTION PILL BADGES IN ELEGANT SOFT ROSE */
.section-badge-pill {
  font-family: var(--font-sans);
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.22em;
  padding: 0.3rem 0.95rem;
  border-radius: 20px;
  margin-bottom: 0.5rem;
  text-transform: uppercase;
}

.rose-pill {
  background-color: rgba(240, 199, 197, 0.45);
  color: #9E4B49;
  border: 1.5px solid #E28E8C;
  box-shadow: 0 2px 8px rgba(240, 199, 197, 0.3);
}

.top-pill {
  display: inline-block;
  margin-bottom: 0.4rem;
}

.couple-names {
  font-family: var(--font-serif);
  font-size: 1.5rem;
  font-weight: 500;
  letter-spacing: 0.22em;
  color: var(--color-brown, #633917);
  text-transform: uppercase;
  margin-bottom: 0.25rem;
}

.header-divider {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  width: 65%;
  margin: 0.4rem auto 1.5rem;
}

.header-divider .line {
  flex: 1;
  height: 1px;
  background: linear-gradient(90deg, transparent, #7F6000, transparent);
}

.header-divider .diamond {
  font-size: 0.65rem;
  color: #E28E8C;
}

.script-title {
  font-family: var(--font-script);
  font-size: 2.9rem;
  font-weight: normal;
  color: var(--color-brown, #633917);
  margin-bottom: 1rem;
  line-height: 1.1;
  text-shadow: 0 1px 2px rgba(255,255,255,0.8);
}

.hero-photo-composition {
  position: relative;
  width: 100%;
  margin-bottom: 1.25rem;
}

.main-photo-wrapper {
  width: 88%;
  margin: 0 auto;
}

.secondary-arch-photo {
  position: absolute;
  bottom: -22px;
  right: 4%;
  width: 44%;
  z-index: 2;
  box-shadow: 0 8px 22px rgba(240, 199, 197, 0.4);
}

.romantic-quote-box {
  padding: 1.25rem 0.5rem;
  margin: 1.25rem 0;
  background: transparent;
  border: none;
  box-shadow: none;
  text-align: center;
}

.quote-text {
  font-family: var(--font-serif);
  font-size: 1.05rem;
  font-weight: 500;
  line-height: 1.65;
  color: var(--color-brown, #633917);
  letter-spacing: 0.01em;
  max-width: 95%;
  margin: 0 auto;
  text-shadow: 0 1px 1px rgba(255, 255, 255, 0.7);
}

.quote-verse {
  font-family: var(--font-serif);
  font-size: 1rem;
  font-weight: 600;
  font-style: italic;
  line-height: 1.6;
  color: var(--color-brown, #633917);
  letter-spacing: 0.01em;
  max-width: 92%;
  margin: 1.25rem auto 0;
  text-shadow: 0 1px 1px rgba(255, 255, 255, 0.7);
}

.guest-pass-card {
  width: 100%;
  border: 2px double var(--color-gold, #7F6000);
  border-radius: 12px;
  padding: 1.1rem;
  background: rgba(255, 255, 255, 0.88);
  box-shadow: 0 4px 15px rgba(127, 96, 0, 0.08);
  text-align: center;
}

.pass-header {
  border-bottom: 1px solid rgba(127, 96, 0, 0.2);
  padding-bottom: 0.4rem;
  margin-bottom: 0.75rem;
}

.pass-title {
  font-family: var(--font-sans);
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.18em;
  color: var(--color-brown, #633917);
}

.guest-family {
  font-family: var(--font-script);
  font-size: 1.9rem;
  color: var(--color-brown, #633917);
  font-weight: normal;
  margin-bottom: 0.6rem;
}

.pass-details {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

.pass-badge {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  padding: 0.35rem 0.85rem;
  border-radius: 6px;
  display: flex;
  align-items: center;
  gap: 0.3rem;
}

.brown-badge {
  background: rgba(99, 57, 23, 0.08);
  border: 1px solid rgba(127, 96, 0, 0.35);
  color: var(--color-brown, #633917);
}

.rose-badge {
  background: rgba(240, 199, 197, 0.45);
  border: 1px solid #E28E8C;
  color: #9E4B49;
  font-weight: 600;
}

.badge-label {
  font-weight: 500;
}

.badge-value {
  font-weight: 700;
}

.parents-grid {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  width: 100%;
}

.parent-card {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  padding: 0.75rem;
  border-radius: 10px;
}

.bride-parents,
.groom-parents {
  background: rgba(99, 57, 23, 0.06);
  border: 1px solid rgba(127, 96, 0, 0.25);
}

.parent-role {
  font-family: var(--font-sans);
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  color: var(--color-brown, #633917);
  margin-bottom: 0.2rem;
}

.parent-name {
  font-family: var(--font-serif);
  font-size: 1.05rem;
  color: var(--color-brown);
}

/* GODPARENTS SECTION */
.godparents-section {
  width: 100%;
}

.godparents-card {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1.25rem 1rem;
  border-radius: 12px;
  background: rgba(99, 57, 23, 0.06);
  border: 1px solid rgba(127, 96, 0, 0.25);
  box-shadow: 0 4px 12px rgba(127, 96, 0, 0.05);
}

.godparent-role {
  font-family: var(--font-sans);
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  color: var(--color-brown, #633917);
  margin-bottom: 0.45rem;
  text-transform: uppercase;
}

.godparent-name {
  font-family: var(--font-serif);
  font-size: 1.15rem;
  font-weight: 600;
  color: var(--color-brown, #633917);
  line-height: 1.35;
}

.godparent-divider {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  width: 45%;
  margin: 0.45rem 0;
}

.godparent-divider .divider-line {
  flex: 1;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(127, 96, 0, 0.4), transparent);
}

.godparent-divider .divider-heart {
  font-size: 0.6rem;
  color: #E28E8C;
}

.date-banner {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  font-family: var(--font-serif);
  color: var(--color-brown);
  margin-bottom: 0.25rem;
}

.date-side {
  flex: 1;
  display: flex;
  align-items: center;
  transform: translateY(12px);
}

.date-side.left-side {
  justify-content: flex-end;
  gap: 0.4rem;
}

.date-side.right-side {
  justify-content: flex-start;
  gap: 0.4rem;
}

.date-day-name, .date-month-name {
  font-size: 1.1rem;
  letter-spacing: 0.12em;
  line-height: 1;
}

.date-day-num {
  font-size: 4.8rem;
  font-weight: 700;
  color: #9E4B49;
  line-height: 1;
  padding: 0 0.5rem;
  flex: 0 0 auto;
  text-align: center;
}

.date-divider {
  color: #E28E8C;
  font-size: 1.4rem;
  line-height: 1;
}

.date-year {
  font-family: var(--font-sans);
  font-size: 2.2rem;
  font-weight: 700;
  letter-spacing: 0.22em;
  color: var(--color-brown);
  text-align: center;
  width: 100%;
  margin-top: 0.2rem;
  margin-bottom: 1.25rem;
}

.full-portrait-space {
  width: 100%;
  margin-top: 1rem;
}

.section-icon {
  margin-bottom: 1rem;
}

.venue-info {
  margin-bottom: 1.25rem;
  width: 100%;
}

.schedule-box {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  padding: 0.75rem;
  background: rgba(255, 255, 255, 0.7);
  border-radius: 10px;
  border: 1px solid rgba(226, 142, 140, 0.4);
  margin-bottom: 1rem;
}

.schedule-item {
  display: flex;
  flex-direction: column;
  gap: 0.15rem;
}

.schedule-divider {
  width: 1px;
  height: 30px;
  background: rgba(127, 96, 0, 0.3);
}

.time-tag {
  font-family: var(--font-sans);
  font-size: 0.9rem;
  font-weight: 700;
  color: var(--color-brown, #633917);
}

.event-label {
  font-family: var(--font-sans);
  font-size: 0.65rem;
  letter-spacing: 0.12em;
  color: var(--color-text-muted);
}

.venue-name {
  font-family: var(--font-serif);
  font-size: 1.3rem;
  color: var(--color-brown);
  margin-bottom: 0.25rem;
}

.venue-address {
  font-family: var(--font-sans);
  font-size: 0.8rem;
  color: var(--color-text-muted);
}

.venue-photo-container {
  width: 100%;
  margin-bottom: 1.25rem;
}

.maps-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.85rem 1.6rem;
  background-color: var(--color-brown, #633917);
  color: #FFFFFF;
  text-decoration: none;
  border-radius: 8px;
  font-family: var(--font-sans);
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.05em;
  transition: all 0.2s ease;
  box-shadow: 0 4px 14px rgba(99, 57, 23, 0.25);
  border: 1.5px solid var(--color-gold, #7F6000);
}

.maps-btn:hover {
  background-color: var(--color-gold, #7F6000);
  color: #FFFFFF;
  transform: translateY(-2px);
}

.dresscode-icons {
  margin-bottom: 0.75rem;
}

.dresscode-type {
  font-family: var(--font-serif);
  font-size: 1.35rem;
  font-weight: 600;
  color: var(--color-brown);
  margin-bottom: 0.5rem;
}

.dresscode-note {
  font-family: var(--font-sans);
  font-size: 0.8rem;
  color: var(--color-text-muted);
  max-width: 88%;
  line-height: 1.45;
}

.monogram-section {
  padding: 0.5rem 0;
}

.monogram-roses-container {
  position: relative;
  width: 100%;
  max-width: 100%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.roses-top-img {
  width: 100%;
  height: auto;
  display: block;
}

.monogram-text {
  font-family: var(--font-script);
  font-size: 3.2rem;
  color: var(--color-brown, #633917);
  position: absolute;
  top: 60%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
  white-space: nowrap;
  text-shadow: 0 1px 4px rgba(253, 251, 247, 0.95), 0 0 10px rgba(253, 251, 247, 0.8);
}

.monogram-verse {
  font-family: var(--font-serif);
  font-size: 1.05rem;
  font-style: italic;
  font-weight: 600;
  color: var(--color-brown, #633917);
  letter-spacing: 0.01em;
  max-width: 90%;
  margin: 1rem auto 0;
  line-height: 1.6;
  text-align: center;
  text-shadow: 0 1px 1px rgba(255, 255, 255, 0.7);
}

.timeline-container {
  position: relative;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin: 1.5rem 0;
}

.timeline-line {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 2px;
  background: linear-gradient(180deg, #E28E8C 0%, #7F6000 50%, #F0C7C5 100%);
  opacity: 0.6;
}

.timeline-item {
  display: flex;
  align-items: center;
  width: 100%;
}

.timeline-item.left {
  justify-content: flex-end;
}

.timeline-item.right {
  justify-content: flex-start;
}

.timeline-content {
  width: 42%;
  display: flex;
  flex-direction: column;
  padding: 0 0.5rem;
}

.timeline-item.left .timeline-content {
  text-align: right;
}

.timeline-item.right .timeline-content {
  text-align: left;
}

.timeline-time {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  font-weight: 700;
  color: var(--color-brown, #633917);
}

.timeline-title {
  font-family: var(--font-sans);
  font-size: 0.65rem;
  letter-spacing: 0.1em;
  color: var(--color-brown);
}

.timeline-node {
  width: 34px;
  height: 34px;
  border-radius: 50%;
  background: var(--color-white);
  border: 2px solid var(--color-gold);
  color: var(--color-brown, #633917);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2;
  box-shadow: 0 3px 8px rgba(0,0,0,0.12);
  margin: 0 0.5rem;
}

.rose-node {
  border-color: #E28E8C;
  color: #9E4B49;
  background: #FDF7F7;
}

.gold-node {
  border-color: var(--color-gold);
  color: var(--color-brown);
}

.bottom-wreath {
  width: 100%;
  max-width: 100%;
  margin: 1.5rem auto 0;
  display: flex;
  justify-content: center;
}

.roses-bottom-img {
  width: 100%;
  height: auto;
  display: block;
}

.gallery-section {
  width: 100%;
}

.gallery-photo-frame {
  width: 100%;
  margin-bottom: 0.75rem;
  overflow: hidden;
  position: relative;
}

.gallery-slide-wrapper {
  width: 100%;
  will-change: transform, opacity;
}

.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: opacity 0.45s cubic-bezier(0.25, 1, 0.5, 1), 
              transform 0.45s cubic-bezier(0.25, 1, 0.5, 1);
}

.slide-fade-enter-from {
  opacity: 0;
  transform: translateX(40px);
}

.slide-fade-leave-to {
  opacity: 0;
  transform: translateX(-40px);
}

.mini-gallery-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0.5rem;
  width: 100%;
  margin-top: 0.5rem;
}

.mini-gallery-item {
  aspect-ratio: 1/1;
  border-radius: 6px;
  overflow: hidden;
  border: 1.5px solid rgba(226, 142, 140, 0.4);
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s, border-color 0.2s;
}

.mini-gallery-item:hover,
.mini-gallery-item.is-active {
  transform: scale(1.06);
  border-color: #9E4B49;
  box-shadow: 0 3px 10px rgba(158, 75, 73, 0.4);
}

.mini-gallery-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* GIFTS SECTION */
.gifts-section {
  width: 100%;
}

.gift-icon-wrap {
  margin-bottom: 0.75rem;
}

.gifts-phrase {
  font-family: var(--font-serif);
  font-size: 1.05rem;
  font-style: italic;
  font-weight: 500;
  line-height: 1.6;
  color: var(--color-brown, #633917);
  letter-spacing: 0.01em;
  max-width: 95%;
  margin: 0 auto 1.5rem;
  text-shadow: 0 1px 1px rgba(255, 255, 255, 0.7);
}

.gift-accounts-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 100%;
}

.account-card {
  width: 100%;
  background: rgba(255, 255, 255, 0.9);
  border: 1px solid rgba(127, 96, 0, 0.28);
  border-radius: 14px;
  padding: 1.1rem;
  box-shadow: 0 4px 15px rgba(127, 96, 0, 0.06);
  text-align: left;
}

.account-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 0.85rem;
  padding-bottom: 0.5rem;
  border-bottom: 1px solid rgba(127, 96, 0, 0.15);
}

.account-badge {
  font-family: var(--font-sans);
  font-size: 0.68rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  padding: 0.25rem 0.65rem;
  border-radius: 6px;
  color: #FFFFFF;
}

.yape-badge {
  background: linear-gradient(135deg, #742284, #873399);
  box-shadow: 0 2px 6px rgba(116, 34, 132, 0.25);
}

.bank-badge {
  background: linear-gradient(135deg, #009B3A, #057831);
  box-shadow: 0 2px 6px rgba(0, 155, 58, 0.25);
}

.account-owner {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  color: var(--color-text-muted, #8C7565);
  font-weight: 500;
}

.account-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.75rem;
}

.account-info {
  display: flex;
  flex-direction: column;
  min-width: 0;
  flex: 1;
}

.account-label {
  font-family: var(--font-sans);
  font-size: 0.68rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--color-text-muted, #8C7565);
  margin-bottom: 0.15rem;
}

.account-number {
  font-family: var(--font-sans);
  font-size: 1.05rem;
  font-weight: 700;
  color: var(--color-brown, #633917);
  letter-spacing: 0.05em;
  word-break: break-all;
}

.account-number.cci-number {
  font-size: 0.84rem;
  letter-spacing: 0.03em;
}

.account-divider {
  height: 1px;
  background: rgba(127, 96, 0, 0.12);
  margin: 0.75rem 0;
}

.copy-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.35rem;
  padding: 0.45rem 0.85rem;
  background: #FAF5EE;
  border: 1px solid rgba(127, 96, 0, 0.5);
  border-radius: 8px;
  color: var(--color-brown, #633917);
  font-family: var(--font-sans);
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.04em;
  cursor: pointer;
  transition: all 0.2s ease;
  flex-shrink: 0;
}

.copy-btn:hover {
  background: #F0E8DC;
  border-color: #7F6000;
  transform: translateY(-1px);
}

.copy-btn:active {
  transform: translateY(0);
}

.copy-btn.is-copied {
  background: #648550;
  border-color: #648550;
  color: #FFFFFF;
}

.copy-feedback,
.copy-action {
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
}

.rsvp-section {
  padding: 1.75rem 1.25rem;
  background: rgba(127, 96, 0, 0.12);
  border-radius: 16px;
  border: 1.5px solid #7F6000;
  box-shadow: inset 0 0 20px rgba(127, 96, 0, 0.1), 0 4px 15px rgba(127, 96, 0, 0.08);
}

.rsvp-text {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  line-height: 1.65;
  letter-spacing: 0.08em;
  color: var(--color-brown);
  margin-bottom: 1.15rem;
  text-transform: uppercase;
}

.planner-info-box {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  padding: 0.75rem 1rem;
  background: rgba(255, 255, 255, 0.85);
  border: 1px dashed #7F6000;
  border-radius: 8px;
  font-family: var(--font-sans);
  font-size: 0.78rem;
  color: var(--color-brown, #633917);
  margin-bottom: 1.25rem;
  line-height: 1.4;
  text-align: center;
}

.planner-info-box strong {
  color: #7F6000;
  font-weight: 700;
}

.rsvp-main-btn {
  padding: 1rem 2.25rem;
  border: none;
  border-radius: 8px;
  background-color: var(--color-brown, #633917);
  color: #FFFFFF;
  font-family: var(--font-sans);
  font-weight: 700;
  font-size: 0.85rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  cursor: pointer;
  box-shadow: 0 4px 15px rgba(99, 57, 23, 0.35);
  transition: all 0.2s ease;
  border: 1.5px solid var(--color-gold, #7F6000);
}

.rsvp-main-btn:hover {
  background-color: var(--color-gold, #7F6000);
  color: #FFFFFF;
  transform: translateY(-2px);
}

.invitation-footer {
  text-align: center;
  padding-top: 1.25rem;
  position: relative;
}

.footer-gold-line {
  width: 50%;
  height: 1px;
  background: linear-gradient(90deg, transparent, #E28E8C, transparent);
  margin: 0 auto 1rem;
}

.footer-blessing {
  font-family: var(--font-serif);
  font-size: 0.95rem;
  color: var(--color-brown);
  letter-spacing: 0.1em;
}

.footer-date {
  font-family: var(--font-sans);
  font-size: 0.75rem;
  color: #9E4B49;
  letter-spacing: 0.2em;
  margin-top: 0.25rem;
}

/* ELEGANT SCROLL REVEAL & PARALLAX ANIMATION */
.reveal-on-scroll {
  opacity: 0;
  transform: translateY(35px) scale(0.97);
  transition: opacity 0.85s cubic-bezier(0.16, 1, 0.3, 1), 
              transform 0.85s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: opacity, transform;
}

.reveal-on-scroll.is-visible {
  opacity: 1;
  transform: translateY(0) scale(1);
}

@media (prefers-reduced-motion: reduce) {
  .reveal-on-scroll {
    opacity: 1 !important;
    transform: none !important;
    transition: none !important;
  }
}
</style>
