<template>
  <!-- Services -->
  <section
    id="services"
    class="relative bg-white px-6 pb-32 pt-16 sm:px-10 lg:px-16"
  >
    <!-- .stage is the size container: every cqw below is % of its width -->
    <div class="stage mx-auto w-full max-w-[1440px]">
      <p class="intro">
        We're a creative technology studio focused on building distinctive
        brands and digital experiences.
      </p>

      <!--
        Stack of real service cards. The two purple strips peeking out above
        the front card are the next cards in line. Click the arrow and the
        front card slides up and fades out, the next one takes its place,
        and the old card goes to the back of the stack.
      -->
      <div class="deck">
        <article
          v-for="(service, i) in services"
          :key="service.id"
          class="card"
          :class="[posClass(i), { 'is-flipping': flipping === i }]"
          :aria-hidden="posOf(i) !== 0"
          :inert="posOf(i) !== 0"
          @animationend="onFlipEnd(i, $event)"
        >
          <div class="copy">
            <h2 class="title">
              <span
                v-for="line in service.title"
                :key="line"
                class="title-line"
              >{{ line }}</span>
            </h2>

            <ul class="tags" aria-label="Capabilities">
              <li v-for="tag in service.tags" :key="tag">
                {{ tag }}
              </li>
            </ul>

            <p class="desc">
              {{ service.description }}
            </p>
          </div>

          <!-- Image stack: back image peeks out behind the front one -->
          <div class="visual" aria-hidden="true">
            <div class="img-back">
              <img
                v-if="service.back"
                :src="service.back"
                alt=""
                draggable="false"
              >
            </div>

            <div class="img-front">
              <img
                v-if="service.front"
                :src="service.front"
                :alt="service.title.join(' ')"
                draggable="false"
              >
            </div>
          </div>
        </article>

        <!-- Next button stays put while the cards flip -->
        <button
          type="button"
          class="next"
          aria-label="Next service"
          @click="next"
        >
          <svg
            class="next-arrow"
            viewBox="0 0 40 16"
            fill="none"
            stroke="currentColor"
            stroke-width="1.4"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
          >
            <path d="M1 8h37M31 1.5 38 8l-7 6.5" />
          </svg>
        </button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

/*
  No images yet, so the cards show a soft placeholder.
  When you have them, import each one and add it to `images`, e.g.:

    import dev1 from '~/assets/services/dev_1.png'
    import dev2 from '~/assets/services/dev_2.png'
    const images = { 'dev_1.png': dev1, 'dev_2.png': dev2 }
*/
const images = {}

function asset(name) {
  return images[name] ?? null
}

// NOTE: only "Smart Development" comes from the design.
// The other two are placeholders, replace the copy with your own.
const services = [
  {
    id: 'development',
    title: ['Smart', 'Development'],
    tags: ['App Development', 'Web Development', 'UI/UX Design', 'Interactions', 'CMS'],
    description:
      'We\'re a creative technology studio focused on building distinctive brands and digital experiences.',
    front: asset('dev_1.png'),
    back: asset('dev_2.png'),
  },
  {
    id: 'branding',
    title: ['Brand', 'Identity'],
    tags: ['Strategy', 'Naming', 'Logo Design', 'Visual Systems', 'Guidelines'],
    description:
      'We\'re a creative technology studio focused on building distinctive brands and digital experiences.',
    front: asset('brand_1.png'),
    back: asset('brand_2.png'),
  },
  {
    id: 'design',
    title: ['Digital', 'Design'],
    tags: ['Web Design', 'Prototyping', 'Motion', 'Design Systems'],
    description:
      'We\'re a creative technology studio focused on building distinctive brands and digital experiences.',
    front: asset('design_1.png'),
    back: asset('design_2.png'),
  },
]

const active = ref(0) // index of the card currently at the front
const flipping = ref(null) // index of the card mid-flip, if any

// 0 = front, 1 = next up, 2 = behind that, ...
function posOf(i) {
  return (i - active.value + services.length) % services.length
}

function posClass(i) {
  const p = posOf(i)
  return p >= 3 ? 'pos-far' : `pos-${p}`
}

function next() {
  if (flipping.value !== null) return // ignore clicks mid-flip

  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches

  if (!reduceMotion) flipping.value = active.value
  active.value = (active.value + 1) % services.length
}

function onFlipEnd(i, e) {
  if (e.target !== e.currentTarget) return
  if (flipping.value === i) flipping.value = null
}
</script>

<style scoped>
/*
  Tweak these to change the look:
  --flip:    how long the slide-up / fade-out takes
  --tilt:    how much the whole deck is rotated
  --radius:  corner radius of the cards
  The peeking strips are the .pos-1 / .pos-2 rules (offset, width, color).
*/
.stage {
  container-type: inline-size;
  --ease: cubic-bezier(0.22, 1, 0.36, 1);
  --flip: 1000ms;
}

/* ---------- Intro line ---------- */
.intro {
  position: relative;
  z-index: 1; /* always stays above the cards */
  max-width: 54cqw;
  font-size: clamp(14px, 2.3cqw, 32px);
  font-weight: 600;
  line-height: 1.1;
  letter-spacing: -0.02em;
  color: #191919;
  text-wrap: balance;
}

/* ---------- Deck (all cards share one grid cell) ---------- */
.deck {
  --tilt: -1.4deg;
  --radius: 3.6cqw;

  position: relative;
  display: grid;
  margin-top: 6.3cqw;
  width: 100%;
  aspect-ratio: 635 / 232;
  transform: rotate(var(--tilt));
  transform-origin: 50% 60%;
}

/* ---------- Cards ---------- */
.card {
  grid-area: 1 / 1;
  position: relative;
  border-radius: var(--radius);
  background-color: var(--bg);
  transform: var(--t);
  transform-origin: 50% 0; /* hinge = top edge */
  z-index: var(--z);
  transition:
    transform 800ms var(--ease),
    background-color 600ms ease,
    opacity 400ms ease;
}

/* Front card */
.pos-0 {
  --t: none;
  --bg: #E8DDF7;
  --z: 4;
}

/* Next card: lighter purple strip peeking above */
.pos-1 {
  --t: translateY(-2.2cqw) rotate(-0.6deg) scaleX(0.945);
  --bg: #D6BDF1;
  --z: 3;
}

/* Card after that: deeper purple strip */
.pos-2 {
  --t: translateY(-4.4cqw) rotate(0.8deg) scaleX(0.894);
  --bg: #C6A2ED;
  --z: 2;
}

/* Any extra cards wait out of sight behind the stack */
.pos-far {
  --t: translateY(-4.4cqw) rotate(0.8deg) scaleX(0.894);
  --bg: #C6A2ED;
  --z: 1;
  opacity: 0;
}

/* ---------- Slide up + fade out ---------- */
.card.is-flipping {
  animation: slide-away var(--flip) cubic-bezier(0.22, 1, 0.36, 1);
}

@keyframes slide-away {
  /* Slide up while fading out... */
  0% {
    transform: none;
    background-color: #E8DDF7;
    opacity: 1;
    z-index: 6;
  }

  55% {
    transform: translateY(-7cqw);
    background-color: #E8DDF7;
    opacity: 0;
    z-index: 6;
  }

  /* ...then quietly take its place at the back of the stack */
  56% {
    transform: var(--t);
    background-color: var(--bg);
    opacity: 0;
    z-index: var(--z);
  }

  100% {
    transform: var(--t);
    background-color: var(--bg);
    opacity: 1;
    z-index: var(--z);
  }
}

/* ---------- Card content: only visible on the front card ---------- */
.copy,
.visual {
  opacity: 0;
  transition: opacity 250ms ease;
}

.pos-0 .copy,
.pos-0 .visual {
  opacity: 1;
  transition-delay: 500ms; /* fade in once the old card has gone */
}

.card.is-flipping .copy,
.card.is-flipping .visual {
  opacity: 1;
  transition: none;
}

.copy {
  position: absolute;
  top: 17%;
  left: 6.3%;
  right: 34%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.title {
  font-size: 7.2cqw;
  font-weight: 600;
  line-height: 0.95;
  letter-spacing: -0.045em;
  color: #0A0A0A;
}

.title-line {
  display: block;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6cqw 2.2cqw;
  margin-top: 3.9cqw;
  font-size: clamp(9px, 1.4cqw, 18px);
  line-height: 1.2;
  color: #191919;
}

.desc {
  max-width: 54cqw;
  margin-top: 3.1cqw;
  font-size: clamp(13px, 2.5cqw, 34px);
  font-weight: 500;
  line-height: 1.12;
  letter-spacing: -0.02em;
  color: #191919;
}

/* ---------- Image stack ---------- */
.visual {
  position: absolute;
  top: 10%;
  left: 68.8%;
  width: 19.5%;
  height: 71.5%;
}

.img-front,
.img-back {
  position: absolute;
  inset: 0;
  overflow: hidden;
  border-radius: 2.2cqw;
  background-color: #D9CFE8; /* shows while images load */
}

.img-front {
  z-index: 2;
  border: max(2px, 0.5cqw) solid #fff;
  transform: rotate(-2.5deg);
}

.img-back {
  background-color: #2B1635;
  border: max(2px, 0.5cqw) solid #fff;
  transform: translate(13%, 6%) rotate(4deg);
}

.img-front img,
.img-back img {
  height: 100%;
  width: 100%;
  object-fit: cover;
  user-select: none;
}

/* ---------- Next button ---------- */
.next {
  position: absolute;
  z-index: 10;
  top: 47%;
  left: 90.9%;
  display: flex;
  align-items: center;
  justify-content: center;
  width: clamp(40px, 6.6cqw, 96px);
  aspect-ratio: 1;
  border-radius: 9999px;
  background-color: #fff;
  color: #0A0A0A;
  box-shadow: 0 4px 18px rgb(60 30 100 / 0.14);
  transform: translate(-50%, -50%);
  transition: transform 300ms var(--ease), box-shadow 300ms ease;
  cursor: pointer;
}

.next:hover {
  transform: translate(-50%, -50%) scale(1.06);
  box-shadow: 0 6px 24px rgb(60 30 100 / 0.22);
}

.next:focus-visible {
  outline: 2px solid #BC96E6;
  outline-offset: 3px;
}

.next-arrow {
  width: 42%;
  height: auto;
  transition: transform 300ms var(--ease);
}

.next:hover .next-arrow {
  transform: translateX(10%);
}

/* ---------- Small screens: stack the card content ---------- */
@container (max-width: 700px) {
  .intro {
    max-width: 85cqw;
  }

  .deck {
    aspect-ratio: auto;
    margin-top: 14cqw;
    --radius: 7cqw;
  }

  .card {
    padding: 8cqw 7cqw 22cqw;
  }

  .copy {
    position: static;
  }

  .title {
    font-size: 11cqw;
  }

  .tags {
    margin-top: 5cqw;
    font-size: 12px;
    gap: 6px 14px;
  }

  .desc {
    margin-top: 5cqw;
    max-width: none;
    font-size: 16px;
  }

  .visual {
    position: relative;
    top: auto;
    left: auto;
    width: 42%;
    height: auto;
    aspect-ratio: 124 / 166;
    margin-top: 8cqw;
  }

  .img-front,
  .img-back {
    border-radius: 4cqw;
  }

  .next {
    top: auto;
    left: auto;
    right: 4%;
    bottom: 5%;
    transform: none;
  }

  .next:hover {
    transform: scale(1.06);
  }
}

@media (prefers-reduced-motion: reduce) {
  .card,
  .copy,
  .visual,
  .next,
  .next-arrow {
    transition: none;
  }

  .card.is-flipping {
    animation: none;
  }
}
</style>