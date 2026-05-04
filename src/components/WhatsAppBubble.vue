<!-- src/components/WhatsAppBubble.vue -->
<template>
  <a 
    :href="whatsappLink" 
    target="_blank" 
    rel="noopener noreferrer"
    class="whatsapp-bubble"
    @click="trackClick"
  >
    <i class="fab fa-whatsapp"></i>
    <span class="bubble-text">Chat with us</span>
  </a>
</template>

<script>
export default {
  name: 'WhatsAppBubble',
  data() {
    return {
      phoneNumber: '2347081123545', // Your WhatsApp number
      message: 'Hello! I have a question about your products.',
      hasBeenClicked: false
    }
  },
  computed: {
    whatsappLink() {
      const encodedMessage = encodeURIComponent(this.message);
      return `https://wa.me/${this.phoneNumber}?text=${encodedMessage}`;
    }
  },
  methods: {
    trackClick() {
      this.hasBeenClicked = true;
      console.log('WhatsApp bubble clicked');
    }
  }
}
</script>

<style scoped>
.whatsapp-bubble {
  position: fixed;
  bottom: 30px;
  right: 30px;
  background-color: #258576; /* Same green as your button-nav */
  color: white !important;
  padding: 12px 20px;
  border-radius: 50px;
  display: flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
  box-shadow: 0 4px 12px rgba(37, 133, 118, 0.3); /* Matching green shadow */
  transition: all 0.3s ease;
  z-index: 1000;
  font-family: 'facee', 'Inter', sans-serif;
  border: 0px;
}

.whatsapp-bubble:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 16px rgba(37, 133, 118, 0.5);
  color: white !important;
  text-decoration: none;
  background-color: #1e6e61; /* Slightly darker on hover like a button */
}

.whatsapp-bubble i {
  font-size: 24px;
  color: white !important;
}

.bubble-text {
  font-size: 16px !important;
  font-weight: 400 !important;
  color: white !important;
}

/* Mobile responsive */
@media (max-width: 768px) {
  .whatsapp-bubble {
    bottom: 20px;
    right: 20px;
    padding: 10px 16px;
  }
  
  .bubble-text {
    display: none; /* Hide text on mobile, show only icon */
  }
  
  .whatsapp-bubble i {
    font-size: 28px;
  }
  
  .whatsapp-bubble {
    padding: 12px;
    border-radius: 50%;
  }
}

/* Animation when page loads */
@keyframes slideIn {
  from {
    transform: translateX(100px);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.whatsapp-bubble {
  animation: slideIn 0.5s ease-out;
}

/* Optional: Subtle pulse to draw attention */
@keyframes pulse {
  0% {
    box-shadow: 0 4px 12px rgba(37, 133, 118, 0.3);
  }
  50% {
    box-shadow: 0 4px 20px rgba(37, 133, 118, 0.6);
  }
  100% {
    box-shadow: 0 4px 12px rgba(37, 133, 118, 0.3);
  }
}

.whatsapp-bubble {
  animation: slideIn 0.5s ease-out, pulse 2s infinite;
  animation-delay: 0s, 0.5s;
}
</style>