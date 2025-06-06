<template>
  <div class="slider-wrapper">
    <div class="slider__text">
      <h2>Explore Architectural Styles</h2>
      <p>Discover beautiful home designs around the world</p>
    </div>

    <div
      class="slider"
      :style="{ transform: `translateX(-${currentSlide * 100}%)` }"
    >
      <div
        v-for="(slide, index) in slides"
        :key="index"
        class="slide"
      >
        <img :src="slide" :alt="`Slide ${index + 1}`" />
      </div>
    </div>

    <button
      class="arrow arrow--left"
      @click="prevSlide"
      :disabled="isAnimating"
      aria-label="Previous Slide"
    >
      &lt;
    </button>
    <button
      class="arrow arrow--right"
      @click="nextSlide"
      :disabled="isAnimating"
      aria-label="Next Slide"
    >
      &gt;
    </button>

    <div class="dots">
      <span
        v-for="(_, index) in slides"
        :key="`dot-${index}`"
        class="dot"
        :class="{ active: index === currentSlide }"
        @click="goToSlide(index)"
        role="button"
        aria-label="Slide dot"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const slides = ref([
  '/img/slide1.png',
  '/img/slide2.png',
  '/img/slide3.png'
]);

const currentSlide = ref(0);
const isAnimating = ref(false);
const animationDuration = 500;
const totalSlides = slides.value.length;

function nextSlide() {
  if (isAnimating.value) return;
  isAnimating.value = true;
  currentSlide.value = (currentSlide.value + 1) % totalSlides;
  setTimeout(() => (isAnimating.value = false), animationDuration);
}

function prevSlide() {
  if (isAnimating.value) return;
  isAnimating.value = true;
  currentSlide.value =
    (currentSlide.value - 1 + totalSlides) % totalSlides;
  setTimeout(() => (isAnimating.value = false), animationDuration);
}

function goToSlide(index: number) {
  if (isAnimating.value || index === currentSlide.value) return;
  isAnimating.value = true;
  currentSlide.value = index;
  setTimeout(() => (isAnimating.value = false), animationDuration);
}
</script>

<style scoped lang="scss">
$color-primary: #007bff;
$font-primary: 'Segoe UI', sans-serif;
$font-secondary: 'Roboto', sans-serif;
$breakpoint-lg: 768px;

.slider-wrapper {
  position: relative;
  width: 90%;
  max-width: 1200px;
  margin: 0 auto;
  overflow: hidden;
  border-radius: 12px;

  .slider {
    display: flex;
    transition: transform 0.5s ease;
    will-change: transform;
  }

  .slide {
    min-width: 100%;
    height: 500px;
    position: relative;
    overflow: hidden;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
    }
  }

  .arrow {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    z-index: 10;
    background-color: $color-primary;
    padding: 12px 18px;
    color: #fff;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    border-radius: 8px;
    user-select: none;
    transition: opacity 0.2s ease;

    &:hover {
      opacity: 0.8;
    }

    &[disabled] {
      opacity: 0.5;
      cursor: not-allowed;
    }

    &--left {
      left: 20px;
    }

    &--right {
      right: 20px;
    }
  }

  .dots {
    position: absolute;
    bottom: 15px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 10px;

    .dot {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: #000000;
      opacity: 0.3;
      cursor: pointer;
      transition: all 0.3s ease;

      &.active {
        background: $color-primary;
        opacity: 1;
      }

      &:hover {
        background: $color-primary;
        opacity: 0.8;
      }
    }
  }
}

.slider__text {
  position: absolute;
  top: 30px;
  left: 40px;
  max-width: 60%;
  z-index: 5;
  color: #000000;
  text-align: left;

  h2 {
    font-family: $font-primary;
    font-size: 32px;
    font-weight: 700;
    margin-bottom: 10px;
  }

  p {
    font-family: $font-secondary;
    font-size: 16px;
    font-weight: 400;
  }

  @media screen and (max-width: $breakpoint-lg) {
    max-width: 90%;
  }
}
</style>
