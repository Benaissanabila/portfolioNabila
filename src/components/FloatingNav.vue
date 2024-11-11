<!-- FloatingNav.vue -->
<template>
    <div class="floating-nav">
      <ul class="floating-nav-list">
        <li v-for="(item, index) in navItems" 
            :key="index" 
            class="floating-nav-item"
            :class="{ 'active': activeSection === item.id }">
          <a @click.prevent="scrollTo(item.id)" 
             :href="`#${item.id}`" 
             class="floating-nav-link">
            <div class="nav-dot">
              <i :class="item.icon"></i>
            </div>
            <span class="nav-tooltip">{{ item.name }}</span>
          </a>
        </li>
      </ul>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'FloatingNav',
    data() {
      return {
        activeSection: 'accueil',
        navItems: [
          { id: 'accueil', name: 'Accueil', icon: 'fas fa-home' },
          { id: 'profil', name: 'Profil', icon: 'fas fa-user' },
          { id: 'competences', name: 'Compétences', icon: 'fas fa-code' },
          { id: 'portfolio', name: 'Portfolio', icon: 'fas fa-briefcase' },
          { id: 'contact', name: 'Contact', icon: 'fas fa-envelope' }
        ]
      }
    },
    mounted() {
      this.observeSections();
    },
    methods: {
      scrollTo(sectionId: string) {
        const element = document.getElementById(sectionId);
        if (element) {
          element.scrollIntoView({ 
            behavior: 'smooth',
            block: 'start'
          });
        }
      },
      observeSections() {
        const observer = new IntersectionObserver(
          (entries) => {
            entries.forEach(entry => {
              if (entry.isIntersecting) {
                this.activeSection = entry.target.id;
              }
            });
          },
          {
            threshold: 0.5,
            rootMargin: '-50% 0px -50% 0px'
          }
        );
  
        this.navItems.forEach(item => {
          const element = document.getElementById(item.id);
          if (element) observer.observe(element);
        });
      }
    }
  });
  </script>
  
  <style scoped>
  .floating-nav {
    position: fixed;
    right: 2rem;
    top: 50%;
    transform: translateY(-50%);
    z-index: 100;
  }
  
  .floating-nav-list {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    background: rgba(255, 255, 255, 0.9);
    padding: 1rem;
    border-radius: 30px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(5px);
  }
  
  .floating-nav-item {
    list-style: none;
  }
  
  .floating-nav-link {
    display: flex;
    align-items: center;
    text-decoration: none;
    position: relative;
  }
  
  .nav-dot {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: white;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
  
  .nav-dot i {
    font-size: 1.2rem;
    color: #6b7280;
    transition: all 0.3s ease;
  }
  
  .nav-tooltip {
    position: absolute;
    right: calc(100% + 10px);
    top: 50%;
    transform: translateY(-50%);
    background: #1f2937;
    color: white;
    padding: 0.5rem 1rem;
    border-radius: 20px;
    font-size: 0.875rem;
    white-space: nowrap;
    opacity: 0;
    visibility: hidden;
    transition: all 0.3s ease;
  }
  
  .nav-tooltip::after {
    content: '';
    position: absolute;
    right: -5px;
    top: 50%;
    transform: translateY(-50%);
    border-left: 5px solid #1f2937;
    border-top: 5px solid transparent;
    border-bottom: 5px solid transparent;
  }
  
  .floating-nav-link:hover .nav-tooltip {
    opacity: 1;
    visibility: visible;
    right: calc(100% + 15px);
  }
  
  .floating-nav-link:hover .nav-dot {
    background: #3b82f6;
  }
  
  .floating-nav-link:hover .nav-dot i {
    color: white;
  }
  
  .floating-nav-item.active .nav-dot {
    background: #3b82f6;
    transform: scale(1.1);
  }
  
  .floating-nav-item.active .nav-dot i {
    color: white;
  }
  
  @media (max-width: 768px) {
    .floating-nav {
      right: 1rem;
    }
    
    .nav-dot {
      width: 32px;
      height: 32px;
    }
    
    .nav-dot i {
      font-size: 1rem;
    }
  }
  </style>