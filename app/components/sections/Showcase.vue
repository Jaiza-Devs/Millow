<template>
  <!-- Showcase -->
  <section
    id="work"
    class="relative bg-white px-6 py-24 sm:px-10 lg:px-16"
  >
    <!-- .stage is the size container: every cqw below is % of its width -->
    <div class="stage mx-auto w-full max-w-[1440px]">
      <div
        class="frame"
        :class="{ 'mid-open': active === 'mid' }"
      >
        <!-- LEFT column: tall image. On hover it shrinks up and a purple text card appears below -->
        <div
          class="col col-left"
          :class="{ 'is-open': active === 'left' }"
          tabindex="0"
          @pointerenter="onEnter('left', $event)"
          @pointerleave="onLeave"
          @pointerup="onTap('left', $event)"
          @focus="active = 'left'"
          @blur="active = null"
        >
          <figure class="tile tile-image">
            <img
              src="~/assets/showcase/showcase_1.png"
              alt="Warm-lit interior with a wooden chair"
              class="media"
              loading="lazy"
              draggable="false"
            >
          </figure>

          <div
            class="tile note note-purple"
            :aria-hidden="active !== 'left'"
          >
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit</p>
          </div>
        </div>

        <!-- MIDDLE: tall video card. On hover it grows to fill the whole frame -->
        <figure
          class="mid"
          tabindex="0"
          @pointerenter="onEnter('mid', $event)"
          @pointerleave="onLeave"
          @pointerup="onTap('mid', $event)"
          @focus="active = 'mid'"
          @blur="active = null"
        >
          <img
            src="~/assets/showcase/showcase_vid_1.png"
            alt="Moisturizer jar resting on driftwood"
            class="media"
            loading="lazy"
            draggable="false"
          >

          <!-- Delete this block if showcase_vid_1.png already has the play icon -->
          <div
            class="pointer-events-none absolute inset-0 flex items-center justify-center"
            aria-hidden="true"
          >
            <svg
              class="play-icon"
              viewBox="0 0 24 24"
              fill="currentColor"
            >
              <path d="M6 3.5v17a1 1 0 0 0 1.52.85l14-8.5a1 1 0 0 0 0-1.7l-14-8.5A1 1 0 0 0 6 3.5Z" />
            </svg>
          </div>
        </figure>

        <!-- RIGHT column: tall image. On hover it shrinks down and a dark text card appears above -->
        <div
          class="col col-right"
          :class="{ 'is-open': active === 'right' }"
          tabindex="0"
          @pointerenter="onEnter('right', $event)"
          @pointerleave="onLeave"
          @pointerup="onTap('right', $event)"
          @focus="active = 'right'"
          @blur="active = null"
        >
          <div
            class="tile note note-dark"
            :aria-hidden="active !== 'right'"
          >
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit</p>
          </div>

          <figure class="tile tile-image">
            <img
              src="~/assets/showcase/showcase_2.png"
              alt="Person walking past a sunlit building"
              class="media"
              loading="lazy"
              draggable="false"
            >
          </figure>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

// Which card is hovered: 'left' | 'mid' | 'right' | null
const active = ref(null)

// Mouse: react to hover. Touch/pen: tap toggles (there is no hover).
function onEnter(slot, e) {
  if (e.pointerType === 'mouse') active.value = slot
}

function onLeave(e) {
  if (e.pointerType === 'mouse') active.value = null
}

function onTap(slot, e) {
  if (e.pointerType !== 'mouse') {
    active.value = active.value === slot ? null : slot
  }
}
</script>

<style scoped>
/*
  Tweak these to change the look:
  --gap:     space between the three columns
  --radius:  corner radius of every card
  --speed:   how long the animations take

  Resting layout = three tall cards, widths 1 : 1.485 : 1.
  Percent values below are the Figma proportions (frame is 1544 x 805):
  side rows split 1.6 : 1 (left) and 1 : 1.58 (right) with a ~4% row gap.
*/
.stage {
  container-type: inline-size;
}

.frame {
  --radius: max(14px, 3.4cqw);
  --speed: 700ms;
  --ease: cubic-bezier(0.22, 1, 0.36, 1);

  position: relative;
  width: 100%;
  aspect-ratio: 1544 / 805;
}

.media {
  position: absolute;
  inset: 0;
  height: 100%;
  width: 100%;
  object-fit: cover;
  user-select: none;
}

/* ---------- Side columns ---------- */
.col {
  position: absolute;
  top: 0;
  bottom: 0;
  outline: none;
  transition:
    opacity 400ms ease,
    transform var(--speed) var(--ease);
}

.col-left  { left: 0;      right: 72.51%; }
.col-right { left: 72.51%; right: 0; }

.tile {
  position: absolute;
  left: 0;
  right: 0;
  overflow: hidden;
  border-radius: var(--radius);
  background-color: #E8E4DC; /* shows while images load */
}

/* Image: full column height at rest */
.tile-image {
  top: 0;
  bottom: 0;
  cursor: pointer;
  transition:
    top var(--speed) var(--ease),
    bottom var(--speed) var(--ease);
}

/* Text cards: hidden at rest, fade/slide in when the column is hovered */
.note {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2cqw;
  text-align: center;
  font-size: clamp(10px, 2.1cqw, 28px);
  font-weight: 500;
  line-height: 1.25;
  opacity: 0;
  pointer-events: none;
  transition:
    opacity 350ms ease,
    transform var(--speed) var(--ease);
}

.note-purple {
  top: 63.08%;
  bottom: 0;
  background-color: #BC96E6;
  color: #191919;
  transform: translateY(8%);
}

.note-dark {
  top: 0;
  bottom: 62.8%;
  background-color: #1E1B1B;
  color: #FFFFFF;
  transform: translateY(-8%);
}

/* Left hovered: image shrinks up, purple card appears below */
.col-left.is-open .tile-image { bottom: 40.95%; }
.col-right.is-open .tile-image { top: 41.23%; }

.col.is-open .note {
  opacity: 1;
  transform: translateY(0);
  transition-delay: 120ms;
}

/* ---------- Middle video card ---------- */
.mid {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 29.59%;
  right: 29.59%;
  z-index: 1;
  overflow: hidden;
  border-radius: var(--radius);
  background-color: #E8E4DC;
  cursor: pointer;
  outline: none;
  transition:
    left var(--speed) var(--ease),
    right var(--speed) var(--ease),
    z-index 0s linear var(--speed); /* drop back only after shrinking */
}

/* Middle hovered: grows to fill the frame, side columns fade away */
.frame.mid-open .mid {
  left: 0;
  right: 0;
  z-index: 10;
  transition:
    left var(--speed) var(--ease),
    right var(--speed) var(--ease),
    z-index 0s;
}

.frame.mid-open .col {
  opacity: 0;
  transform: scale(0.96);
  pointer-events: none;
}

/* ---------- Play icon ---------- */
.play-icon {
  width: clamp(36px, 5.5cqw, 72px);
  height: auto;
  color: #fff;
  opacity: 0.95;
  filter: drop-shadow(0 2px 8px rgb(0 0 0 / 0.35));
}

@media (prefers-reduced-motion: reduce) {
  .col,
  .tile-image,
  .note,
  .mid,
  .frame.mid-open .mid {
    transition: none;
  }
}
</style>