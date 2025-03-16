<!-- components/PizzaCard.vue -->
<template>
    <div class="bg-yellow-50 rounded-lg p-4 relative overflow-hidden">
      <!-- Badge (Popular ou Highly Rated) -->
      <div class="absolute top-2 left-2 z-10">
        <span 
          v-if="isPopular" 
          class="bg-yellow-400 text-xs font-semibold px-2 py-1 rounded-full text-black"
        >
          Popular
        </span>
        <span 
          v-if="isHighlyRated" 
          class="bg-green-400 text-xs font-semibold px-2 py-1 rounded-full text-black"
        >
          Highly Rated
        </span>
      </div>
      
      <!-- Image de la pizza -->
      <div class="flex justify-center items-center mb-3">
        <img 
          :src="imgSrc" 
          :alt="title" 
          class="w-32 h-32 object-cover rounded-full"
        />
      </div>
      
      <!-- Titre de la pizza -->
      <h3 class="text-center font-semibold text-sm">{{ title }}</h3>
      
      <!-- Prix et bouton d'ajout -->
      <div class="flex justify-between items-center mt-2">
        <div class="flex items-center gap-1">
          <span class="text-sm font-semibold">{{ price }}</span>
          <span v-if="originalPrice" class="text-xs text-gray-400 line-through">{{ originalPrice }}</span>
        </div>
        
        <!-- Badge avec le nombre (comme dans l'image) -->
        <div v-if="badgeNumber" class="bg-green-800 w-6 h-6 rounded-full flex items-center justify-center">
          <span class="text-white text-xs">{{ badgeNumber }}</span>
        </div>
        
        <!-- Bouton d'ajout au panier (alternative) -->
        <button 
          v-else
          @click="addToCart"
          class="bg-green-800 w-6 h-6 rounded-full flex items-center justify-center text-white font-bold"
        >
          +
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  const props = defineProps({
    title: {
      type: String,
      required: true
    },
    price: {
      type: String,
      required: true
    },
    originalPrice: {
      type: String,
      default: ''
    },
    imgSrc: {
      type: String,
      required: true
    },
    isPopular: {
      type: Boolean,
      default: false
    },
    isHighlyRated: {
      type: Boolean,
      default: false
    },
    badgeNumber: {
      type: [String, Number],
      default: ''
    }
  });
  
  const emit = defineEmits(['add-to-cart']);
  
  const addToCart = () => {
    emit('add-to-cart', {
      id: props.id || props.title,
      title: props.title,
      price: props.price,
      imgSrc: props.imgSrc
    });
  };
  </script>
  
  <style scoped>
  /* Vous pouvez ajouter des styles spécifiques au composant ici si nécessaire */
  </style>
  