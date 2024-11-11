<template>
    <div class="animated-background" @mousemove="handleMouseMove">
      <!-- Fond avec triangles connectés -->
      <canvas ref="canvas" class="particles-canvas"></canvas>
  
      <!-- Contenu principal -->
      <section class="home-section" >
        <div class="intro">
          <!-- Nom avec effet de machine à écrire -->
          <h1 class="gradient-text">
            <span class="firstname" v-html="displayedFirstName"></span>
            <span class="lastname" v-html="displayedLastName"></span>
            <span class="cursor" :class="{ 'blink': isFirstNameComplete && isLastNameComplete }">|</span>
          </h1>
          
          <!-- Titre avec effet de machine à écrire -->
          <h2 class="title" v-html="displayedTitle"></h2>
          
          <p>Passionnée par le développement web, je crée des solutions innovantes et performantes.</p>
          <button @click="showPortfolio" class="discover-button">
            Découvrez mes réalisations
          </button>
        </div>
  
        <div class="about-me" id="profil">
          <h2>Qui suis-je ?</h2>
          <p>
            Je suis une développeuse web passionnée, spécialisée dans les technologies Full Stack.
            J'aime relever des défis techniques et travailler sur des projets créatifs.
            J'ai acquis des compétences solides en développement front-end et back-end et
            je suis toujours à la recherche de nouvelles opportunités d'apprentissage.
          </p>
        </div>
      </section>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref, onMounted, onUnmounted } from 'vue';
  
  interface Point {
    x: number;
    y: number;
    vx: number;
    vy: number;
    angle: number; // Ajout de l'angle pour la rotation des triangles
  }
  
  interface MousePosition {
    x: number;
    y: number;
  }
  
  export default defineComponent({
    name: 'AnimatedBackground',
    setup() {
      const canvas = ref<HTMLCanvasElement | null>(null);
      const points = ref<Point[]>([]);
      const mousePos = ref<MousePosition>({ x: 0, y: 0 });
      let animationFrameId: number;
      let ctx: CanvasRenderingContext2D | null = null;
  
      // Variables pour l'animation du texte
      const firstName = "Nabila";
      const lastName = "Benaissa";
      const title = "Développeuse Web Junior Full Stack";
      
      const displayedFirstName = ref('');
      const displayedLastName = ref('');
      const displayedTitle = ref('');
      
      const isFirstNameComplete = ref(false);
      const isLastNameComplete = ref(false);
      const isTitleComplete = ref(false);
  
      // Initialiser les points (triangles)
      const initPoints = () => {
        const numberOfPoints = 50;
        points.value = Array.from({ length: numberOfPoints }, () => ({
          x: Math.random() * window.innerWidth,
          y: Math.random() * window.innerHeight,
          vx: (Math.random() - 0.5) * 2,
          vy: (Math.random() - 0.5) * 2,
          angle: Math.random() * Math.PI * 1 // Angle initial aléatoire
        }));
      };
  
      // Dessiner un triangle
      const drawTriangle = (x: number, y: number, size: number, angle: number) => {
        if (!ctx) return;
  
        ctx.save();
        ctx.translate(x, y);
        ctx.rotate(angle);
        
        ctx.beginPath();
        ctx.moveTo(0, -size);
        ctx.lineTo(-size, size);
        ctx.lineTo(size, size);
        ctx.closePath();
        
        ctx.fillStyle = '#65bcc8';
        ctx.fill();
        
        ctx.restore();
      };
  
      // Dessiner les connections et triangles
      const drawConnections = () => {
        if (!ctx || !canvas.value) return;
  
        ctx.clearRect(0, 0, canvas.value.width, canvas.value.height);
        
        // Dessiner les connections
        ctx.beginPath();
        ctx.strokeStyle = 'rgba(101, 188, 200, 0.1)';
        
        points.value.forEach((point, i) => {
          points.value.slice(i + 1).forEach(otherPoint => {
            const distance = Math.hypot(point.x - otherPoint.x, point.y - otherPoint.y);
            if (distance < 150) {
              ctx.moveTo(point.x, point.y);
              ctx.lineTo(otherPoint.x, otherPoint.y);
            }
          });
        });
        ctx.stroke();
  
        // Dessiner les triangles
        points.value.forEach(point => {
          drawTriangle(point.x, point.y, 3, point.angle);
        });
      };
  
      // Mettre à jour les positions
      const updatePoints = () => {
        if (!canvas.value) return;
  
        points.value = points.value.map(point => {
          let newX = point.x + point.vx;
          let newY = point.y + point.vy;
          let newAngle = point.angle + 0.02; // Rotation continue des triangles
  
          // Rebondir sur les bords
          if (newX < 0 || newX > canvas.value!.width) {
            point.vx *= -1;
            newAngle += Math.PI / 4; // Changement d'angle au rebond
          }
          if (newY < 0 || newY > canvas.value!.height) {
            point.vy *= -1;
            newAngle += Math.PI / 4; // Changement d'angle au rebond
          }
  
          // Effet de répulsion
          const dx = mousePos.value.x - point.x;
          const dy = mousePos.value.y - point.y;
          const distance = Math.hypot(dx, dy);
  
          if (distance < 100) {
            const force = (200 - distance) / 10;
            newX -= (dx / distance) * force * 5;
            newY -= (dy / distance) * force * 5;
            newAngle += force * 0.1; // Rotation plus rapide près de la souris
          }
  
          return {
            ...point,
            x: newX,
            y: newY,
            angle: newAngle
          };
        });
  
        drawConnections();
        animationFrameId = requestAnimationFrame(updatePoints);
      };
  
      // Effet de frappe
      const typeText = async () => {
        // Taper le prénom
        for (let i = 0; i < firstName.length; i++) {
          displayedFirstName.value = firstName.substring(0, i + 1);
          await new Promise(resolve => setTimeout(resolve, 150));
        }
        isFirstNameComplete.value = true;
        await new Promise(resolve => setTimeout(resolve, 500));
  
        // Taper le nom
        for (let i = 0; i < lastName.length; i++) {
          displayedLastName.value = lastName.substring(0, i + 1);
          await new Promise(resolve => setTimeout(resolve, 150));
        }
        isLastNameComplete.value = true;
        await new Promise(resolve => setTimeout(resolve, 1000));
  
        // Taper le titre
        for (let i = 0; i < title.length; i++) {
          displayedTitle.value = title.substring(0, i + 1);
          await new Promise(resolve => setTimeout(resolve, 100));
        }
        isTitleComplete.value = true;
      };
  
      // Gérer le mouvement de la souris
      const handleMouseMove = (event: MouseEvent) => {
        mousePos.value = {
          x: event.clientX,
          y: event.clientY
        };
      };
  
      // Gérer le redimensionnement
      const handleResize = () => {
        if (!canvas.value) return;
        canvas.value.width = window.innerWidth;
        canvas.value.height = window.innerHeight;
      };
  
      // Navigation
      const showPortfolio = () => {
        window.location.href = '#portfolio';
      };
  
      onMounted(() => {
        if (!canvas.value) return;
        
        ctx = canvas.value.getContext('2d');
        handleResize();
        window.addEventListener('resize', handleResize);
        
        initPoints();
        updatePoints();
        typeText();
      });
  
      onUnmounted(() => {
        if (animationFrameId) {
          cancelAnimationFrame(animationFrameId);
        }
        window.removeEventListener('resize', handleResize);
      });
  
      return {
        canvas,
        handleMouseMove,
        showPortfolio,
        displayedFirstName,
        displayedLastName,
        displayedTitle,
        isFirstNameComplete,
        isLastNameComplete
      };
    }
  });
  </script>
  
  <style scoped>
  /* Import des polices Google Fonts */
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@700&family=Roboto:wght@500&display=swap');
  
  .animated-background {
    position: relative;
    min-height: 100vh;
    overflow: hidden;
    background: linear-gradient(to bottom right, #1a1a2e, #16213e);
  }
  
  .particles-canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
  }
  
  .home-section {
    position: relative;
    z-index: 2;
    padding: 2rem;
    text-align: center;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
  }
  
  .intro {
    margin-bottom: 2rem;
  }
  
  .gradient-text {
    font-family: 'Poppins', sans-serif;
    background: linear-gradient(45deg, #004d6e, #65bcc8, #d0ff00);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    font-size: 4rem;
    font-weight: 700;
    text-align: center;
    letter-spacing: 2px;
    text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
    display: flex;
    justify-content: center;
    gap: 1rem; /* Espace entre prénom et nom */
    flex-wrap: wrap;
  }
  
  .firstname, .lastname {
    display: inline-block;
  }
  
  .cursor {
    display: inline-block;
    color: #65bcc8;
    font-weight: normal;
    margin-left: 2px;
  }
  
  .cursor.blink {
    animation: blink 1s step-end infinite;
  }
  
  @keyframes blink {
    from, to { color: transparent }
    50% { color: #65bcc8 }
  }
  
  .title {
    font-family: 'Roboto', sans-serif;
    font-size: 2rem;
    margin: 1.5rem 0;
    color: #65bcc8;
    font-weight: 500;
    letter-spacing: 0.5px;
  }
  
  .intro p {
    font-size: 1.2rem;
    color: rgba(255, 255, 255, 0.8);
    margin: 1rem 0;
    font-family: 'Roboto', sans-serif;
  }
  
  .discover-button {
    margin-top: 1rem;
    padding: 0.8rem 2rem;
    background-color: #00bcd4;
    color: white;
    border: none;
    font-size: 1.1rem;
    cursor: pointer;
    border-radius: 5px;
    transition: background-color 0.3s, transform 0.2s;
    font-family: 'Roboto', sans-serif;
  }
  
  .discover-button:hover {
    background-color: #77d2de;
    transform: translateY(-2px);
  }
  
  .about-me {
    margin-top: 5rem;
    background-color: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    padding: 3rem;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    max-width: 800px;
    width: 90%;
  }
  
  .about-me h2 {
    font-size: 2rem;
    color: white;
    margin-bottom: 1.5rem;
    font-family: 'Poppins', sans-serif;
  }
  
  .about-me p {
    font-size: 1.1rem;
    color: rgba(255, 255, 255, 0.9);
    line-height: 1.6;
    font-family: 'Roboto', sans-serif;
  }
  
  @media (max-width: 768px) {
    .gradient-text {
      font-size: 3rem;
    }
  
    .title {
      font-size: 1.5rem;
    }
  
    .about-me {
      padding: 2rem;
    }
  }
  </style>