<script setup>
import { images } from "./data.js";

import { ref, watchEffect } from "vue";

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
  <div
    @scroll="rueda"
    ref="slider"
    class="snap-y snap-mandatory h-screen overflow-hidden scroll-smooth"
  >
    <div class="snap-start h-screen" v-for="(image, i) in images" :key="i">
      <img
        class="w-full h-full object-cover object-top"
        :src="image.url[0]"
        alt=""
      />
    </div>
  </div>
  <div
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

    <button class="" @click="bajar">
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
  </div>
</template>

<style scoped></style>
