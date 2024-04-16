<template>
  <div>
    <h1 style="text-align: center">Dog API Carousel</h1>
    <DogApiCarousel />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import DogApiCarousel from "./components/DogApiCarousel.vue";

export default defineComponent({
  name: "App",
  components: {
    DogApiCarousel,
  },
});
</script>
