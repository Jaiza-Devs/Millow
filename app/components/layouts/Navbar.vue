<template>
  <header
    class="fixed inset-x-0 top-0 z-50 transition-all duration-300"
    :class="isScrolled || isMenuOpen
      ? 'border-b border-black/5 bg-[#FAF9F6]/95 shadow-sm backdrop-blur-md'
      : 'bg-transparent'"
  >
    <nav
      class="mx-auto flex h-20 max-w-[1440px] items-center justify-between px-6 sm:px-10 lg:px-16"
      aria-label="Main navigation"
    >
      <!-- Logo -->
      <NuxtLink
        to="/"
        class="relative z-50 inline-flex items-center"
        aria-label="Millow home"
        @click="closeMenu"
      >
        <img
          :src="millowLogo"
          alt="Millow"
          class="h-[37.78px] w-[119px] object-contain"
        >
      </NuxtLink>

      <!-- Desktop Navigation -->
      <div class="hidden items-center gap-10 md:flex">
        <a href="#work" class="nav-link">
          <span class="nav-label">Work</span>
          <span class="nav-indicator" aria-hidden="true" />
        </a>

        <a href="#studio" class="nav-link">
          <span class="nav-label">Studio</span>
          <span class="nav-indicator" aria-hidden="true" />
        </a>

        <a href="#contact" class="nav-link">
          <span class="nav-label">Contact</span>
          <span class="nav-indicator" aria-hidden="true" />
        </a>
      </div>

      <!-- Mobile Menu Button -->
      <button
        type="button"
        class="relative z-50 flex h-10 w-10 flex-col items-center justify-center gap-[6px] md:hidden"
        :aria-expanded="isMenuOpen"
        aria-label="Toggle navigation menu"
        @click="toggleMenu"
      >
        <span
          class="h-0.5 w-6 bg-[#191919] transition-all duration-300"
          :class="isMenuOpen ? 'translate-y-[4px] rotate-45' : ''"
        />

        <span
          class="h-0.5 w-6 bg-[#191919] transition-all duration-300"
          :class="isMenuOpen ? '-translate-y-[4px] -rotate-45' : ''"
        />
      </button>
    </nav>

    <!-- Mobile Navigation -->
    <Transition
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="opacity-0 -translate-y-2"
      enter-to-class="translate-y-0 opacity-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="translate-y-0 opacity-100"
      leave-to-class="-translate-y-2 opacity-0"
    >
      <div
        v-if="isMenuOpen"
        class="absolute inset-x-0 top-full border-b border-black/5 bg-[#FAF9F6] px-6 pb-8 pt-4 md:hidden"
      >
        <nav class="flex flex-col" aria-label="Mobile navigation">
          <a
            href="#about"
            class="border-b border-black/5 py-4 text-lg text-[#191919] transition-colors hover:text-[#A67C52]"
            @click="closeMenu"
          >
            About
          </a>

          <a
            href="#services"
            class="border-b border-black/5 py-4 text-lg text-[#191919] transition-colors hover:text-[#A67C52]"
            @click="closeMenu"
          >
            Services
          </a>

          <a
            href="#process"
            class="border-b border-black/5 py-4 text-lg text-[#191919] transition-colors hover:text-[#A67C52]"
            @click="closeMenu"
          >
            Process
          </a>
        </nav>
      </div>
    </Transition>
  </header>
</template>

<script setup>
import { onMounted, onBeforeUnmount, ref } from 'vue'
import millowLogo from '~/assets/images/millow_word.png'

const isScrolled = ref(false)
const isMenuOpen = ref(false)

function handleScroll() {
  isScrolled.value = window.scrollY > 20
}

function toggleMenu() {
  isMenuOpen.value = !isMenuOpen.value
}

function closeMenu() {
  isMenuOpen.value = false
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })
  handleScroll()
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
/*
  Tweak these two to change the feel:
  --lift: how far the label moves up on hover
  --gap:  space between the label and the square
*/
.nav-link {
  --lift: 12px;
  --gap: 6px;

  position: relative;
  display: inline-flex;
  align-items: center;
  font-size: 22px;
  line-height: 1.2;
  color: #55534F;
  transition: color 200ms ease;
}

.nav-link:hover,
.nav-link:focus-visible {
  color: #191919;
}

/* Label rests vertically centered in the navbar, then lifts on hover */
.nav-label {
  display: inline-block;
  transition: transform 250ms cubic-bezier(0.22, 1, 0.36, 1);
}

.nav-link:hover .nav-label,
.nav-link:focus-visible .nav-label {
  transform: translateY(calc(var(--lift) * -1));
}

/* Square sits below the label's lifted position and pops in */
.nav-indicator {
  position: absolute;
  top: calc(100% - var(--lift) + var(--gap));
  left: 50%;
  width: 24px;
  height: 24px;
  border-radius: 4px;
  background-color: #BC96E6;
  opacity: 0;
  transform: translateX(-50%) scale(0);
  transform-origin: center;
  transition:
    opacity 200ms ease,
    transform 250ms cubic-bezier(0.22, 1, 0.36, 1);
  pointer-events: none;
}

.nav-link:hover .nav-indicator,
.nav-link:focus-visible .nav-indicator {
  opacity: 1;
  transform: translateX(-50%) scale(1);
}

@media (prefers-reduced-motion: reduce) {
  .nav-link,
  .nav-label,
  .nav-indicator {
    transition: none;
  }
}
</style>