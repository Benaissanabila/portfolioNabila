<template>
    <nav class="navbar" id="accueil">
      <ul class="navbar-list">
        <li class="navbar-item"><a @click.prevent="scrollTo('accueil')" href="#accueil">Accueil</a></li>
        <li class="navbar-item"><a @click.prevent="scrollTo('profil')" href="#profil">Profil</a></li>
        <li class="navbar-item"><a @click.prevent="scrollTo('competences')" href="#competences">Compétences</a></li>
        <li class="navbar-item"><a @click.prevent="scrollTo('portfolio')" href="#portfolio">Portfolio</a></li>
        <li class="navbar-item"><a @click.prevent="scrollTo('contact')" href="#contact">Contact</a></li>
      </ul>
    </nav>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'Navbar',
    methods: {
        scrollTo(elementId: string) {
  const element = document.getElementById(elementId);
  if (element) {
    const offset = 80; // Ajustez selon la hauteur de votre navbar
    const elementPosition = element.getBoundingClientRect().top;
    const offsetPosition = elementPosition + window.pageYOffset - offset;

    window.scrollTo({
      top: offsetPosition,
      behavior: 'smooth'
    });
  }
}
    }
  });
  </script>
  
  <style scoped>
  .navbar {
    display: flex;
    justify-content: center;
    background-color: #02709f;
    padding: 1rem;
  }
  
  .navbar-list {
    display: flex;
    gap: 2rem;
    list-style-type: none;
    padding: 0;
    margin: 0;
  }
  
  .navbar-item a {
    color: #fff;
    text-decoration: none;
    font-size: 1.2rem;
    font-weight: bold;
    transition: color 0.3s;
  }
  
  .navbar-item a:hover {
    color: #77d2de;
  }
  </style>
  