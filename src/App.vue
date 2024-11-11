<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import Navbar from './components/Navbar.vue';
import HomeSection from './components/HomeSection.vue';
import AboutMe from './components/AboutMe.vue';
import FloatingNav from './components/FloatingNav.vue';

// État réactif pour l'affichage de FloatingNav
const showFloatingNav = ref(false);

// Méthode pour observer la section Profil
function handleScroll() {
  const profilSection = document.getElementById('profil');
  if (profilSection) {
    const rect = profilSection.getBoundingClientRect();
    // Affiche FloatingNav si la section Profil est dans la vue
    showFloatingNav.value = rect.top >= 0 && rect.bottom <= window.innerHeight;
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
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
  <RouterView />
</template>

<style scoped>
/* Ajoutez vos styles ici */
</style>
