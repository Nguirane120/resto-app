<!-- components/OrderItem.vue (suite) -->
<template>
    <div class="py-3 border-b border-gray-100 last:border-0">
      <div class="flex items-start">
        <!-- Image de l'article -->
        <div class="w-16 h-16 flex-shrink-0">
          <img 
            :src="imgSrc" 
            :alt="name"
            class="w-full h-full object-cover rounded-lg"
            @error="useDefaultImage"
          />
        </div>
        
        <!-- Détails de l'article -->
        <div class="ml-3 flex-grow">
          <div class="flex justify-between">
            <h3 class="font-medium text-sm">{{ name }}</h3>
            <span class="text-sm font-semibold">{{ formattedPrice }}</span>
          </div>
          
          <!-- Toppings et sauces -->
          <p v-if="toppings" class="text-xs text-gray-500 mt-1">{{ toppings }}</p>
          <p v-if="sauces" class="text-xs text-gray-500 mt-0.5">{{ sauces }}</p>
          
          <!-- Contrôles de quantité -->
          <div class="flex items-center justify-end mt-2">
            <button 
              @click="decreaseQuantity" 
              class="w-6 h-6 flex items-center justify-center text-gray-400 hover:text-primary transition-colors"
              :disabled="quantity <= 1"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 12H4" />
              </svg>
            </button>
            
            <span class="mx-2 text-sm font-medium w-4 text-center">{{ quantity }}</span>
            
            <button 
              @click="increaseQuantity" 
              class="w-6 h-6 flex items-center justify-center text-gray-400 hover:text-primary transition-colors"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" />
              </svg>
            </button>
            
            <button 
              @click="removeItem" 
              class="ml-3 text-gray-400 hover:text-red-500 transition-colors"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const props = defineProps({
    id: {
      type: [String, Number],
 
    },
    name: {
      type: String,
    
    },
    price: {
      type: Number,
    },
    quantity: {
      type: Number,
      default: 1
    },
    toppings: {
      type: String,
      default: ''
    },
    sauces: {
      type: String,
      default: ''
    },
    imgSrc: {
      type: String,
    }
  });
  
  const defaultImage = 'https://via.placeholder.com/150?text=Food';
  const useDefaultImage = (event) => {
    event.target.src = defaultImage;
  };
  
  const emit = defineEmits(['update:quantity', 'remove']);
  
  const increaseQuantity = () => {
    emit('update:quantity', {
      id: props.id,
      quantity: props.quantity + 1
    });
  };
  
  const decreaseQuantity = () => {
    if (props.quantity > 1) {
      emit('update:quantity', {
        id: props.id,
        quantity: props.quantity - 1
      });
    }
  };
  
  const removeItem = () => {
    emit('remove', props.id);
  };
  
  // Formater le prix avec le symbole € si nécessaire
  const formattedPrice = computed(() => {
    if (typeof props.price === 'number') {
      return `€${props.price.toFixed(2)}`;
    }
    
    // Si c'est déjà une chaîne, vérifier si elle contient déjà le symbole €
    if (props.price?.includes('€')) {
      return props.price;
    }
    
    return `€${props.price}`;
  });
  </script>