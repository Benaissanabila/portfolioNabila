<template>
    <div class="contact-form-container" id="contact">
      <h2 class="form-title">Contactez-moi</h2>
      
      <form @submit.prevent="handleSubmit" class="contact-form">
        <div class="form-group">
          <label for="name">Nom:</label>
          <input 
            type="text" 
            id="name" 
            v-model="formData.name"
            required
            class="form-input"
            placeholder="Votre nom"
          >
        </div>
  
        <div class="form-group">
          <label for="email">Email:</label>
          <input 
            type="email" 
            id="email" 
            v-model="formData.email"
            required
            class="form-input"
            placeholder="votre@email.com"
          >
        </div>
  
        <div class="form-group">
          <label for="subject">Sujet:</label>
          <input 
            type="text" 
            id="subject" 
            v-model="formData.subject"
            required
            class="form-input"
            placeholder="Sujet de votre message"
          >
        </div>
  
        <div class="form-group">
          <label for="message">Message:</label>
          <textarea 
            id="message" 
            v-model="formData.message"
            required
            class="form-textarea"
            placeholder="Votre message..."
            rows="5"
          ></textarea>
        </div>
  
        <button type="submit" class="submit-button" :disabled="isSubmitting">
          {{ isSubmitting ? 'Envoi en cours...' : 'Envoyer' }}
        </button>
  
        <div v-if="submitStatus.show" :class="['status-message', submitStatus.isError ? 'error' : 'success']">
          {{ submitStatus.message }}
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import { defineComponent } from 'vue';
  const BREVO_API_KEY = import.meta.env.VITE_BREVO_API_KEY;
const RECIPIENT_EMAIL = import.meta.env.VITE_RECIPIENT_EMAIL;

console.log('API Key:', BREVO_API_KEY);
console.log('Recipient Email:', RECIPIENT_EMAIL);
  
  export default defineComponent({
    name: 'Contact',
    data() {
      return {
        formData: {
          name: '',
          email: '',
          subject: '',
          message: ''
        },
        isSubmitting: false,
        submitStatus: {
          show: false,
          message: '',
          isError: false
        }
      };
    },
    methods: {
      async handleSubmit() {
        this.isSubmitting = true;
        this.submitStatus.show = false;
  
        // Création de l'instance axios avec les headers par défaut
        const axiosInstance = axios.create({
          headers: {
            'Content-Type': 'application/json',
            'api-key': BREVO_API_KEY
          }
        });
  
        try {
          const response = await axiosInstance.post('https://api.brevo.com/v3/smtp/email', {
            sender: {
              name: this.formData.name,
              email: RECIPIENT_EMAIL
            },
            to: [{
              email: RECIPIENT_EMAIL ,
              name: 'Destinataire'
            }],
            subject: this.formData.subject,
            htmlContent: `
              <h3>Nouveau message de contact</h3>
              <p><strong>De:</strong> ${this.formData.name} (${this.formData.email})</p>
              <p><strong>Sujet:</strong> ${this.formData.subject}</p>
              <p><strong>Message:</strong></p>
              <p>${this.formData.message.replace(/\n/g, '<br>')}</p>
            `
          });
  
          if (response.status === 201) {
            // Réinitialiser le formulaire
            this.formData = {
              name: '',
              email: '',
              subject: '',
              message: ''
            };
            
            this.submitStatus = {
              show: true,
              message: 'Message envoyé avec succès!',
              isError: false
            };
          }
        } catch (error) {
          console.error('BREVO ERROR:', error);
          console.log('Configuration complète :', error.config);
          console.log('En-têtes envoyés :', error.config.headers);
  
          // Message d'erreur plus détaillé
          let errorMessage = 'Une erreur est survenue. Veuillez réessayer.';
          if (error.response) {
            if (error.response.status === 401) {
              errorMessage = 'Erreur d\'authentification. Veuillez vérifier la configuration.';
            } else if (error.response.data && error.response.data.message) {
              errorMessage = error.response.data.message;
            }
          }
          
          this.submitStatus = {
            show: true,
            message: errorMessage,
            isError: true
          };
        } finally {
          this.isSubmitting = false;
        }
      }
    }
  });
  </script>
  
  <style scoped>
  .contact-form-container {
  max-width: 600px;
  margin: 10rem auto;
  padding: 2rem;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  border-radius: 1rem;
  box-shadow: 
    0 8px 32px rgba(0, 0, 0, 0.1),
    0 2px 8px rgba(0, 0, 0, 0.05);
  transition: transform 0.3s ease;
}

.contact-form-container:hover {
  transform: translateY(-2px);
}

.form-title {
  font-size: 2.5rem;
  color: #2d3436;
  margin-bottom: 2rem;
  text-align: center;
  background: linear-gradient(45deg, #6c5ce7, #a8e6cf);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.form-group {
  margin-bottom: 1.5rem;
  position: relative;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 100;
  color: #2d3436;
  transform-origin: left;
  transition: transform 0.3s ease;
}

.form-input,
.form-textarea {
  width: 95%;
  padding: 1rem;
  border: 2px solid #e0e0e0;
  border-radius: 0.5rem;
  background: rgba(255, 255, 255, 0.9);
  transition: all 0.3s ease;
  font-size: 1rem;
}

.form-input:focus,
.form-textarea:focus {
  outline: none;
  border-color: #6c5ce7;
  box-shadow: 0 0 0 4px rgba(108, 92, 231, 0.1);
  background: white;
}

.form-input::placeholder,
.form-textarea::placeholder {
  color: #b2bec3;
  transition: opacity 0.3s ease;
}

.form-input:focus::placeholder,
.form-textarea:focus::placeholder {
  opacity: 0;
}

.submit-button {
  width: 100%;
  padding: 1rem 2rem;
  background: linear-gradient(45deg, #6c5ce7, #a8e6cf);
  border: none;
  border-radius: 0.5rem;
  color: white;
  font-weight: 600;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.submit-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.2),
    transparent
  );
  transition: left 0.7s ease;
}

.submit-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(108, 92, 231, 0.4);
}

.submit-button:hover::before {
  left: 100%;
}

.submit-button:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.status-message {
  margin-top: 1rem;
  padding: 1rem;
  border-radius: 0.5rem;
  text-align: center;
  font-weight: 500;
  animation: slideIn 0.3s ease;
}

.status-message.success {
  background: rgba(168, 230, 207, 0.2);
  color: #2d3436;
  border: 1px solid #a8e6cf;
}

.status-message.error {
  background: rgba(255, 107, 107, 0.2);
  color: #2d3436;
  border: 1px solid #ff6b6b;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Animations des champs */
@keyframes focusField {
  0% { transform: scale(1); }
  50% { transform: scale(1.02); }
  100% { transform: scale(1); }
}

.form-input:focus,
.form-textarea:focus {
  animation: focusField 0.3s ease;
}

/* Media Queries pour la responsivité */
@media (max-width: 768px) {
  .contact-form-container {
    margin: 1rem;
    padding: 1.5rem;
  }

  .form-title {
    font-size: 2rem;
  }
}

/* Dark mode support */
@media (prefers-color-scheme: dark) {
  .contact-form-container {
    background: rgba(45, 52, 54, 0.8);
  }

  .form-title {
    background: linear-gradient(45deg, #a8e6cf, #6c5ce7);
    -webkit-background-clip: text;
    background-clip: text;
  }

  .form-group label {
    color: #dfe6e9;
  }

  .form-input,
  .form-textarea {
    background: rgba(45, 52, 54, 0.9);
    border-color: #636e72;
    color: #dfe6e9;
  }

  .form-input::placeholder,
  .form-textarea::placeholder {
    color: #636e72;
  }

  .status-message.success {
    background: rgba(168, 230, 207, 0.1);
    color: #dfe6e9;
  }

  .status-message.error {
    background: rgba(255, 107, 107, 0.1);
    color: #dfe6e9;
  }
}
  </style>
  