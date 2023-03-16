<script setup>
import { images } from "./data.js";

import { ref } from "vue";

const position = ref(0);
const slider = ref();

const bajar = () => {
  const largo = slider.value.clientHeight;
  slider.value.scrollTop += largo;

  if (position.value === images.length - 1) {
    slider.value.scrollTop = 0;
  }
};

const click = (i) => {
  position.value = i;
  const largo = slider.value.clientHeight;
  slider.value.scrollTop = largo * i;
};

const rueda = () => {
  const largo = slider.value.clientHeight;
  images.forEach((_, i) => {
    if (slider.value.scrollTop >= largo * i) {
      position.value = i;
    }
  });
};
</script>

<template>
  <main
    @scroll="rueda"
    ref="slider"
    class="snap-y snap-mandatory h-screen overflow-hidden scroll-smooth relative"
  >
    <div
      class="snap-start h-screen relative"
      v-for="(image, i) in images"
      :key="i"
    >
      <div
        class="absolute bottom-0 bg-gradient-to-t from-black/80 to-transparent w-full h-96"
      ></div>
      <div class="absolute bottom-0 max-w-2xl md:p-16 p-8">
        <h1 class="text-white text-4xl">{{ image.title }}</h1>
      </div>
      <img
        class="w-full h-full object-cover object-top"
        :src="image.url[0]"
        alt=""
      />
    </div>
  </main>
  <nav
    class="absolute top-0 right-10 h-screen z-50 flex flex-col justify-center gap-8"
  >
    <div class="flex flex-col items-center gap-2">
      <button
        :class="position === i ? 'bg-white w-4 h-4' : 'w-2 h-2'"
        class="bg-white/50 rounded-full transition-all"
        v-for="(_, i) in images"
        @click="click(i)"
      ></button>
    </div>

    <button @click="bajar">
      <svg
        class="fill-white/80 w-10 h-10"
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
      >
        <path
          d="m18.707 12.707-1.414-1.414L13 15.586V6h-2v9.586l-4.293-4.293-1.414 1.414L12 19.414z"
        ></path>
      </svg>
    </button>
  </nav>
</template>

<style scoped></style>
