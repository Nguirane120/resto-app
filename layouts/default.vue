<!-- layouts/default.vue -->
<template>
    <div class="min-h-screen bg-gray-50 font-sans flex flex-col md:flex-row">
      <!-- Contenu principal -->
      <div class="flex-1 overflow-auto pb-16 md:pb-0">
        <slot />
      </div>
      
      <!-- Barre latérale de commande (visible uniquement sur les grands écrans) -->
      <div class="hidden md:block w-full max-w-xs xl:max-w-sm border-l">
        <div class="sticky top-0 p-4">
          <OrderSidebar />
        </div>
      </div>
      
      <!-- Barre de navigation mobile (visible uniquement sur petit écran) -->
      <div class="md:hidden fixed bottom-0 left-0 right-0 bg-white border-t shadow-lg z-50">
        <div class="flex justify-around items-center">
          <button class="flex flex-col items-center justify-center p-3 flex-1">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
            </svg>
            <span class="text-xs mt-1">Home</span>
          </button>
          
          <button class="flex flex-col items-center justify-center p-3 flex-1">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
            <span class="text-xs mt-1">Search</span>
          </button>
          
          <button class="flex flex-col items-center justify-center p-3 flex-1 relative" @click="showMobileCart = !showMobileCart">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
            </svg>
            <span class="text-xs mt-1">Cart</span>
            <span v-if="cartCount > 0" class="absolute -top-1 -right-1 bg-primary text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center">
              {{ cartCount }}
            </span>
          </button>
          
          <button class="flex flex-col items-center justify-center p-3 flex-1">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
            </svg>
            <span class="text-xs mt-1">Account</span>
          </button>
        </div>
      </div>
      
      <!-- Panier mobile (s'affiche quand on clique sur l'icône panier) -->
      <div 
        v-if="showMobileCart" 
        class="fixed inset-0 bg-black bg-opacity-50 z-50 md:hidden"
        @click.self="showMobileCart = false"
      >
        <div class="absolute bottom-0 left-0 right-0 bg-white rounded-t-xl p-4 max-h-[80vh] overflow-y-auto">
          <div class="flex justify-between items-center mb-4">
            <h2 class="text-xl font-bold">Your Order</h2>
            <button @click="showMobileCart = false" class="p-2">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
          
          <OrderSidebar />
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { OrderSidebar } from '#components';
import { ref } from 'vue';
//   import { storeToRefs } from 'pinia';
//   import { useCartStore } from '~/stores/cart';
  
  const showMobileCart = ref(false);
//   const cartStore = useCartStore();
//   const { totalItems } = storeToRefs(cartStore);
  
//   // Nombre d'articles dans le panier
  const cartCount = 2;
  </script>