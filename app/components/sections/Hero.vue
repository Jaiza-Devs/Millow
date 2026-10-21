<template>
  <!-- Hero -->
  <section
    id="home"
    class="relative flex min-h-screen items-center justify-center overflow-hidden bg-white px-6 pb-32 pt-48"
  >
    <!-- Everything below is sized in em so spacing scales with the headline -->
    <div
      class="flex flex-col items-center text-center text-[clamp(2.75rem,8vw,114.99px)]"
    >
      <h1
        class="text-[1em] font-semibold leading-[0.92] tracking-[-0.04em] text-[#0A0A0A]"
      >
        <!-- Mascot sits just past the "g", rising above the cap height and
             dipping down to roughly the x-height of "Creating" -->
        <span class="relative inline-block">
          Creating
          <img
            src="~/assets/images/millow_mini.png"
            alt=""
            aria-hidden="true"
            class="absolute left-full top-[-0.25em] ml-[-0.40em] h-[0.7914em] w-[0.9566em] max-w-none select-none [image-rendering:pixelated]"
            draggable="false"
          >
        </span>
        <br>
        something
        <br>
        <span class="text-[#BC96E6]">uniquely</span>
        yours.
      </h1>

      <a
        href="#studio"
        aria-label="Scroll down"
        class="mt-[1em] inline-block transition-transform duration-300 hover:translate-y-1"
      >
        <img
          src="~/assets/images/arrow_down.png"
          alt=""
          aria-hidden="true"
          class="h-[0.5em] w-auto select-none"
          draggable="false"
        >
      </a>
    </div>
  </section>
</template>