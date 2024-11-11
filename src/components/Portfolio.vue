<template>
    <section class="portfolio-section" id="portfolio">
      <h2>Mon Portfolio</h2>
      <div class="portfolio-wrapper">
        <div class="portfolio-images">
          <img v-for="(image, index) in images" :key="index" :src="image" alt="Portfolio Image" />
        </div>
      </div>
    </section>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'Portfolio',
    data() {
      return {
        images: [
          '/images/project1.jpg',
          '/images/projects2.png',
          '/images/projects3.png',
          '/images/projects4.png',
          '/images/project5.jpg'
        ]
      };
    }
  });
  </script>
  
  <style scoped>
  /* Section Styling */
  .portfolio-section {
    padding: 2rem;
    background: linear-gradient(45deg, #004d6e, #65bcc8, #f0f8cd);
    color: white;
    text-align: center;
    border-radius: 8px;
  }
  
  h2 {
    font-size: 2rem;
    margin-bottom: 1.5rem;
  }
  
  /* Wrapper for portfolio images */
  .portfolio-wrapper {
    overflow: hidden;
    width: 100%;
    position: relative;
  }
  
  .portfolio-images {
    display: flex;
    animation: scrollImages 20s linear infinite;
  }
  
  .portfolio-images img {
    max-width: 50%;
    height: auto;
    margin-right: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }
  
  /* Animation for scrolling images */
  @keyframes scrollImages {
    0% {
      transform: translateX(100%);
    }
    100% {
      transform: translateX(-100%);
    }
  }
  </style>
  