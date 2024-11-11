<template>
    <section class="competences-section" id="competences">
      <h2>Mes Compétences</h2>
      <div class="competences-grid">
        <div v-for="(competence, index) in competences" :key="index" class="competence-card">
          <div class="icon">
            <i :class="competence.icon"></i>
          </div>
          <h3>{{ competence.title }}</h3>
          <ul>
            <li v-for="(item, i) in competence.items" :key="i">
              <i :class="item.icon" class="item-icon"></i> <!-- Affichage de l'icône de l'item -->
              {{ item.name }}
            </li>
          </ul>
        </div>
      </div>
    </section>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'Competences',
    data() {
      return {
        competences: [
          { 
            title: 'Langages', 
            icon: 'fas fa-code', 
            items: [
              { name: 'JavaScript', icon: 'fab fa-js' },
              { name: 'TypeScript', icon: 'fab fa-js-square' },
              { name: 'HTML5', icon: 'fab fa-html5' },
              { name: 'CSS', icon: 'fab fa-css3-alt' }
            ]
          },
          { 
            title: 'Frameworks', 
            icon: 'fas fa-layer-group', 
            items: [
              { name: 'Vue.js', icon: 'fab fa-vuejs' },
              { name: 'React.js', icon: 'fab fa-react' }
            ]
          },
          { 
            title: 'Bases de données', 
            icon: 'fas fa-database', 
            items: [
              { name: 'MySQL', icon: 'fas fa-database' },
              { name: 'MongoDB', icon: 'fas fa-database' }
            ]
          },
          { 
            title: 'Outils', 
            icon: 'fas fa-tools', 
            items: [
              { name: 'Visual Studio', icon: 'fas fa-code' },
              { name: 'WebStorm', icon: 'fas fa-code' },
              { name: 'GitHub', icon: 'fab fa-github' },
              { name: 'Jira', icon: 'fab fa-jira' },
              { name: 'Figma', icon: 'fab fa-figma' },
              { name: 'Cypress', icon: 'fas fa-vial' }
            ]
          }
        ]
      };
    }
  });
  </script>
  
  <style scoped>
  /* Section Styling */
  .competences-section {
    padding: 2rem;
    background: linear-gradient(to right, #02709f, #00bcd4);
    color: white;
    border-radius: 8px;
    text-align: center;
  }
  
  h2 {
    font-size: 2rem;
    margin-bottom: 1.5rem;
  }
  
  /* Competences Grid */
  .competences-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
  }
  
  /* Card Styling */
  .competence-card {
    background-color: white;
    color: #02709f;
    border-radius: 8px;
    padding: 1rem;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
    transform: perspective(1px) translateZ(0);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .competence-card:hover {
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }
  
  /* Icon Styling */
  .icon {
    font-size: 4rem;
    margin-bottom: 0.5rem;
    color: #00bcd4;
    transition: transform 0.3s ease;
  }
  
  .competence-card:hover .icon {
    transform: rotate(360deg);
  }
  
  /* List Styling */
  .competence-card ul {
    list-style: none;
    padding: 0;
    font-size: 0.9rem;
    color: #333;
   
  }
  
  .competence-card li {
    padding: 0.2rem 0;
    display: flex;
    align-items: center;
  }
  
  .item-icon {
    margin-right: 1rem;
    color: #00bcd4;
    font-size: 2rem;
  }
  </style>
  