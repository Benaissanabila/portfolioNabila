<template>
    <div class="about-section">
      <div class="tags-container">
        <div v-for="(tag, index) in tags" 
             :key="index" 
             class="tag-card"
             :class="`tag-card-${index + 1}`">
          <div class="tag-content">
            <div class="tag-header">
              <div class="icon" :class="tag.iconClass">
                <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
                <i :class="tag.icon"></i>
              </div>
              <h3>{{ tag.title }}</h3>
            </div>
            <p>{{ tag.description }}</p>
            <div class="tag-hover-effect">
              <i class="fas fa-arrow-right"></i>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'AboutMe',
    data() {
      return {
        tags: [
          {
            title: "Le goût de l'apprentissage",
            description: "Autodidacte, je renforce mes connaissances et ma pratique en établissant une veille régulière et en me formant par différentes ressources.",
            icon: 'fas fa-graduation-cap',
            iconClass: 'learning'
          },
          {
            title: "Donner vie à un projet",
            description: "Le numérique offre la possibilité de transformer une simple idée en un projet concret. C'est un aspect que j'apprécie particulièrement dans ce domaine.",
            icon: 'fas fa-rocket',
            iconClass: 'project'
          },
          {
            title: "Rien n'est impossible",
            description: "Face aux problèmes, j'aime réfléchir et observer les différentes solutions possibles, en autonomie ou avec l'aide d'une communauté.",
            icon: 'fas fa-lightbulb',
            iconClass: 'solution'
          }
        ]
      }
    }
  });
  </script>
  
  <style scoped>
  .about-section {
    width: 100%;
    padding: 4rem 2rem;
    background: #f8fafc;
  }
  
  .tags-container {
    max-width: 1200px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
  }
  
  .tag-card {
    position: relative;
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
    overflow: hidden;
    height: 100%;
  }
  
  .tag-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  }
  
  .tag-content {
    padding: 2rem;
    height: 100%;
    display: flex;
    flex-direction: column;
  }
  
  .tag-header {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
  
  .icon {
    width: 48px;
    height: 48px;
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.5rem;
    transition: all 0.3s ease;
  }
  
  .tag-card:hover .icon {
    transform: scale(1.1);
  }
  
  /* Couleurs spécifiques pour chaque icône */
  .learning {
    background: rgba(59, 130, 246, 0.1);
    color: #3b82f6;
  }
  
  .project {
    background: rgba(16, 185, 129, 0.1);
    color: #10b981;
  }
  
  .solution {
    background: rgba(245, 158, 11, 0.1);
    color: #f59e0b;
  }
  
  h3 {
    font-size: 1.25rem;
    font-weight: 600;
    color: #1f2937;
    margin: 0;
  }
  
  p {
    color: #6b7280;
    line-height: 1.7;
    font-size: 1rem;
    margin-top: 0.5rem;
  }
  
  .tag-hover-effect {
    position: absolute;
    bottom: 1.5rem;
    right: 1.5rem;
    opacity: 0;
    transition: all 0.5s ease;
  }
  
  .tag-card:hover .tag-hover-effect {
    opacity: 2;
    transform: translateX(10px);
  }
  
  /* Animation d'entrée des cartes */
  .tag-card {
    animation: fadeInUp 0.8s ease-out forwards;
    opacity: 0;
  }
  
  .tag-card-1 { animation-delay: 0.1s; }
  .tag-card-2 { animation-delay: 0.3s; }
  .tag-card-3 { animation-delay: 0.5s; }
  
  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  /* Responsive */
  @media (max-width: 768px) {
    .about-section {
      padding: 2rem 1rem;
    }
    
    .tags-container {
      grid-template-columns: 1fr;
    }
    
    .tag-card {
      margin-bottom: 1rem;
    }
  }
  </style>