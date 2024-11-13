<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import Navbar from './components/Navbar.vue';
import HomeSection from './components/HomeSection.vue';
import AboutMe from './components/AboutMe.vue';
import FloatingNav from './components/FloatingNav.vue';
import Competences from './components/Competence.vue';
import Portfolio from './components/Portfolio.vue';
import Contact from './components/Contact.vue';
// État réactif pour l'affichage de FloatingNav
const showFloatingNav = ref(false);

function handleScroll() {
  const accueilSection = document.getElementById('accueil');
  if (accueilSection) {
    const rect = accueilSection.getBoundingClientRect();
    // Affiche FloatingNav si l’utilisateur a dépassé la section Accueil
    showFloatingNav.value = rect.bottom <= 0;
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  handleScroll(); 
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});
</script>

<template>
  <Navbar/>
  <FloatingNav v-show="showFloatingNav" />
  <HomeSection/>
  <AboutMe/>
  <Competences/>
  <Portfolio/>
  <Contact/>
  <RouterView />
</template>

<style scoped>
/* Ajoutez vos styles ici */
</style>
