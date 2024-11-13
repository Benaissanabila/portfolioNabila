<template>
    <section class="portfolio-section" id="portfolio">
      <h2>Mon Portfolio</h2>
      <div class="portfolio-grid">
        <div 
          v-for="(project, index) in projects" 
          :key="index"
          class="project-card"
          @mousemove="handleMouseMove($event, index)"
          @mouseleave="handleMouseLeave(index)"
          :style="getCardTransform(index)"
        >
          <div class="card-content">
            <div class="image-container">
              <img :src="project.image" :alt="project.title" />
              <div class="project-status" :class="project.status">
                {{ project.status }}
              </div>
            </div>
            <div class="project-info">
              <div class="project-header">
                <h3>{{ project.title }}</h3>
                <span class="project-date">{{ project.date }}</span>
              </div>
              <p class="project-description">{{ project.description }}</p>
              <div class="project-details">
                <div class="tech-stack">
                  <span v-for="(tech, techIndex) in project.technologies" 
                        :key="techIndex" 
                        class="tech-tag">
                    {{ tech }}
                  </span>
                </div>
                <div class="project-links">
                  <a v-if="project.github" :href="project.github" class="link-icon" target="_blank">
                    <i class="fab fa-github"></i>
                  </a>
                  <a v-if="project.live" :href="project.live" class="link-icon" target="_blank">
                    <i class="fas fa-external-link-alt"></i>
                  </a>
                </div>
              </div>
             
            </div>
          </div>
          <div class="card-shine" :style="getShineEffect(index)"></div>
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
        projects: [
          {
            image: '/images/projects1.png',
            title: 'Portfolio 3D',
            description: 'Portfolio interactif avec animations 3D',
            technologies: ['Vue.js', 'TypeScript','HTML','CSS'],
            status: 'completed',
            date: 'Nov 2024',
            github: 'https://github.com/Benaissanabila/portfolioNabila',
            transform: { x: 0, y: 0, shine: 0 }
          },
          {
            image: '/images/projects2.png',
            title: 'Foodies ',
            description: 'Application Full Stack pour rechercher des restaurants et reserver une table.',
            technologies: ['Vue.js', 'MongoDB','HTML','CSS', 'TypeScript','API'],
            status: 'in-progress',
            date: 'Sep 2024',
            github: 'https://github.com/Benaissanabila/FoodiesNP',
            transform: { x: 0, y: 0, shine: 0 }
          },
          {
            image: '/images/projects3.png',
            title: 'Zen Café',
            description: 'Projet scolaire ,Application pour un Café',
            technologies: ['HTML', 'CSS'],
            status: 'completed',
            date: 'Nov 2023',
            transform: { x: 0, y: 0, shine: 0 }
          },
          {
            image: '/images/projects4.png',
            title: 'Weather App',
            description: 'Application météo avec prévisions détaillées',
            technologies: ['React', 'AWS', 'OpenWeather API'],
            status: 'completed',
            date: 'Oct 2023',
            github: 'https://github.com/user/project7',
            transform: { x: 0, y: 0, shine: 0 }
          },
          {
            image: '/images/projects5.png',
            title: 'Music Player',
            description: 'Lecteur de musique avec visualisations audio',
            technologies: ['React', 'spotify API','GraphQL', 'AWS','TypeScript','HTML','CSS'],
            status: 'completed',
            date: 'Juin 2024',
            github: 'https://github.com/Benaissanabila/musify',
            transform: { x: 0, y: 0, shine: 0 }
          },
          {
            image: '/images/projects6.png',
            title: 'Ledor',
            description: 'Projet scolaire Application pour agence immobilier  ',
            technologies: ['HTML', 'CSS'],
            status: 'Completed',
            date: 'Mars 2024',
            transform: { x: 0, y: 0, shine: 0 }
          },
        //   {
        //     image: '/images/projects7.png',
        //     title: 'Weather App',
        //     description: 'Application météo avec prévisions détaillées',
        //     technologies: ['React', 'Redux', 'OpenWeather API'],
        //     status: 'completed',
        //     date: 'Oct 2023',
        //     github: 'https://github.com/user/project7',
        //     live: 'https://project7.com',
        //     transform: { x: 0, y: 0, shine: 0 }
        //   },
        //   {
        //     image: '/images/projects8.png',
        //     title: 'Chat Application',
        //     description: 'Application de chat en temps réel multiplateforme',
        //     technologies: ['Socket.io', 'Express', 'MongoDB'],
        //     status: 'completed',
        //     date: 'Sept 2023',
        //     github: 'https://github.com/user/project8',
        //     transform: { x: 0, y: 0, shine: 0 }
        //   },
        //   {
        //     image: '/images/projects9.png',
        //     title: "Gestion d'une Liste de Client",
        //     description: 'Lecteur de musique avec visualisations audio',
        //     technologies: ['Vue.js', 'Web Audio API', 'Electron'],
        //     status: 'in-progress',
        //     date: 'Mai 2024',
        //     github: 'https://github.com/user/project9',
        //     transform: { x: 0, y: 0, shine: 0 }
        //   }
        ]
      };
    },
    methods: {
      handleMouseMove(event: MouseEvent, index: number) {
        const card = event.currentTarget as HTMLElement;
        const rect = card.getBoundingClientRect();
        const x = event.clientX - rect.left;
        const y = event.clientY - rect.top;
        
        const rotateY = ((x / rect.width) - 0.5) * 15;
        const rotateX = ((y / rect.height) - 0.5) * -15;
        
        const shine = Math.atan2(y - rect.height / 2, x - rect.width / 2);
        
        this.projects[index].transform = {
          x: rotateX,
          y: rotateY,
          shine: shine
        };
      },
      handleMouseLeave(index: number) {
        this.projects[index].transform = {
          x: 0,
          y: 0,
          shine: 0
        };
      },
      getCardTransform(index: number) {
        const { x, y } = this.projects[index].transform;
        return {
          transform: `perspective(1000px) rotateX(${x}deg) rotateY(${y}deg)`,
          transition: 'transform 0.2s ease-out'
        };
      },
      getShineEffect(index: number) {
        const { shine } = this.projects[index].transform;
        return {
          background: `linear-gradient(${shine}rad, rgba(255,255,255,0.2), transparent 70%)`,
          opacity: shine !== 0 ? 1 : 0
        };
      },
      openProject(index: number) {
        console.log(`Ouverture du projet ${index + 1}`);
      }
    }
  });
  </script>
  
  <style scoped>
  .portfolio-section {
    padding: 4rem 2rem;
    background: linear-gradient(135deg, #0f172a, #1e293b);
    min-height: 100vh;
  }
  
  h2 {
    font-size: 3rem;
    color: #e2e8f0;
    text-align: center;
    margin-bottom: 3rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 3px;
    background: linear-gradient(to right, #60a5fa, #3b82f6);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  
  .portfolio-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
    gap: 2.5rem;
    max-width: 1600px;
    margin: 0 auto;
    padding: 1rem;
  }
  
  .project-card {
    position: relative;
    background: rgba(255, 255, 255, 0.05);
    border-radius: 24px;
    overflow: hidden;
    cursor: pointer;
    backdrop-filter: blur(10px);
    transform-style: preserve-3d;
    padding: 1.5rem;
    border: 1px solid rgba(255, 255, 255, 0.1);
  }
  
  .image-container {
    position: relative;
    overflow: hidden;
    border-radius: 16px;
  }
  
  .project-card img {
    width: 100%;
    height: 220px;
    object-fit: cover;
    transform: scale(1);
    transition: transform 0.5s ease;
  }
  
  .project-card:hover img {
    transform: scale(1.05);
  }
  
  .project-status {
    position: absolute;
    top: 1rem;
    right: 1rem;
    padding: 0.5rem 1rem;
    border-radius: 20px;
    font-size: 0.8rem;
    font-weight: 600;
    text-transform: uppercase;
  }
  
  .project-status.completed {
    background: rgba(34, 197, 94, 0.2);
    color: #4ade80;
  }
  
  .project-status.in-progress {
    background: rgba(234, 179, 8, 0.2);
    color: #fde047;
  }
  
  .project-info {
    padding: 1.5rem 0.5rem;
    color: #e2e8f0;
  }
  
  .project-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
  }
  
  .project-header h3 {
    font-size: 1.5rem;
    font-weight: 600;
    color: #f8fafc;
  }
  
  .project-date {
    font-size: 0.9rem;
    color: #94a3b8;
  }
  
  .project-description {
    font-size: 1rem;
    color: #cbd5e1;
    line-height: 1.6;
    margin-bottom: 1.5rem;
  }
  
  .project-details {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
  }
  
  .tech-stack {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  
  .tech-tag {
    background: rgba(99, 102, 241, 0.1);
    padding: 0.4rem 0.8rem;
    border-radius: 12px;
    font-size: 0.8rem;
    color: #818cf8;
    border: 1px solid rgba(99, 102, 241, 0.2);
  }
  
  .project-links {
    display: flex;
    gap: 1rem;
  }
  
  .link-icon {
    color: #94a3b8;
    font-size: 1.2rem;
    transition: color 0.3s ease;
  }
  
  .link-icon:hover {
    color: #60a5fa;
  }
  
  .view-project {
    background: linear-gradient(135deg, #3b82f6, #1d4ed8);
    border: none;
    padding: 0.8rem 1.8rem;
    border-radius: 12px;
    color: white;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    gap: 0.8rem;
    width: 100%;
    justify-content: center;
  }
  
  .view-project:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
  }
  
  .arrow {
    transition: transform 0.3s ease;
    font-size: 1.2rem;
  }
  
  .view-project:hover .arrow {
    transform: translateX(5px);
  }
  
  .card-shine {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    pointer-events: none;
    transition: opacity 0.3s ease;
    border-radius: 24px;
  }
  
  @media (max-width: 768px) {
    .portfolio-grid {
      grid-template-columns: 1fr;
    }
    
    .project-card {
      transform: none !important;
    }
    
    h2 {
      font-size: 2.5rem;
    }
  }
  
  @media (min-width: 1600px) {
    .portfolio-grid {
      grid-template-columns: repeat(3, 1fr);
    }
  }
  </style>