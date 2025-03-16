<!-- components/AppHeader.vue -->
<template>
    <div class="bg-gradient-to-r from-[#e74c3c] to-[#e74c3c] text-white p-4 relative overflow-hidden">
      <div class="flex flex-col md:flex-row justify-between items-center">
        <!-- Texte promotionnel -->
        <div class="z-10 mb-4 md:mb-0">
          <h1 class="text-2xl md:text-3xl font-bold leading-tight">
            Discount up to 20% for<br>Garlic Prawn Pizza
          </h1>
          <p class="mt-2 text-sm md:text-base">
            Enjoy the delicious Garlic Prawn Pizza at a special price!
          </p>
          <p class="text-xs mt-1 text-yellow-200">
            Until 01.01.2026
          </p>
        </div>
        
        <!-- Image de la pizza avec badges -->
        <div class="relative">
          <img 
            src="https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg" 
            alt="Garlic Prawn Pizza" 
            class="w-32 h-32 md:w-40 md:h-40 object-cover rounded-full shadow-lg z-10"
          />
          
          <!-- Badge "SPECIAL TODAY" -->
          <div class="absolute -top-2 -left-4 bg-pizza-yellow text-black font-semibold text-xs md:text-sm rounded-full px-2 py-1 transform rotate-12 shadow">
            SPECIAL TODAY
          </div>
          
          <!-- Badge "20% OFF" -->
          <div class="absolute -bottom-2 -right-2 bg-pizza-yellow text-black rounded-full flex flex-col items-center justify-center w-12 h-12 md:w-14 md:h-14 text-center transform rotate-12 shadow">
            <span class="text-xs font-medium">Disc. up to</span>
            <span class="font-bold text-sm md:text-base">20%</span>
          </div>
        </div>
      </div>
      
      <!-- Éléments décoratifs -->
      <div class="absolute -bottom-6 -left-6 w-16 h-16 bg-white opacity-10 rounded-full"></div>
      <div class="absolute top-4 right-8 w-8 h-8 bg-white opacity-10 rounded-full"></div>
      <div class="absolute bottom-8 right-16 w-4 h-4 bg-white opacity-10 rounded-full"></div>
    </div>
  </template>