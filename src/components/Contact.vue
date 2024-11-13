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
  
  const BREVO_API_KEY = import.meta.env.VITE_BREVO_API_KEY;
const RECIPIENT_EMAIL = import.meta.env.VITE_RECIPIENT_EMAIL;

console.log('API Key:', BREVO_API_KEY);
console.log('Recipient Email:', RECIPIENT_EMAIL);
  
  export default {
    name: 'ContactForm',
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
  };
  </script>
  
  <style scoped>
  /* Ajoute ici tes styles si nécessaire */
  </style>
  