<template>
  <div class="dog-api-carousel">
    <button @click="prevSlide" class="carousel-arrow prev">‹</button>
    <q-carousel
      control-color="white"
      arrows-color="white"
      v-model="selectedSlideIndex"
      controls
    >
      <template v-if="images.length > 0">
        <q-carousel-slide
          v-for="(imageUrl, index) in images"
          :key="index"
          :name="index"
          class="carousel-slide"
        >
          <img class="carousel-image" :src="imageUrl" alt="Dog Image" />
        </q-carousel-slide>
      </template>
      <div v-else class="error-message">
        {{ error || "Error fetching dog images." }}
      </div>
    </q-carousel>
    <button @click="nextSlide" class="carousel-arrow next">›</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";
import axios from "axios";

export default defineComponent({
  name: "DogApiCarousel",
  setup() {
    const images = ref<string[]>([]);
    const selectedSlideIndex = ref(0);
    const error = ref<string | null>(null);

    const prevSlide = () => {
      selectedSlideIndex.value--;
      if (selectedSlideIndex.value < 0) {
        selectedSlideIndex.value = images.value.length - 1;
      }
    };

    const nextSlide = () => {
      selectedSlideIndex.value++;
      if (selectedSlideIndex.value >= images.value.length) {
        selectedSlideIndex.value = 0;
      }
    };

    onMounted(async () => {
      try {
        const response = await axios.get(
          "https://api.thedogapi.com/v1/images/search?limit=10"
        );
        console.log(response);
        images.value = response.data.map((item: { url: string }) => item.url);
      } catch (err) {
        console.error("Error fetching dog images:", err);
        error.value = err.message || "Error fetching dog images.";
      }
    });

    return { images, selectedSlideIndex, error, prevSlide, nextSlide };
  },
});
</script>

<style scoped>
.dog-api-carousel {
  max-width: 600px;
  margin: 0 auto;
  position: relative;
}

.carousel-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: transparent;
  border: none;
  font-size: 2rem;
  cursor: pointer;
  padding: 10px;
  color: black;
}

.prev {
  left: 10px;
}

.next {
  right: 10px;
}

.carousel-slide {
  overflow: hidden;
  position: relative;
}

.carousel-image {
  width: 100%;
  height: auto;
  position: absolute;
  top: 0;
  left: 0;
}
</style>
