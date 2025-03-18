<template>
  <header class="bg-white shadow-sm p-4 flex justify-between items-center">
    <!-- Bouton Hamburger - Visible seulement sur mobile -->
    <button 
      @click="$emit('toggleSidebar')" 
      class="block md:hidden text-gray-700 focus:outline-none mr-4"
    >
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
      </svg>
    </button>

    <!-- Titre de la page -->
    <h2 class="text-xl font-semibold flex-1">{{ pageTitle }}</h2>
    
    <!-- Profile Icon & Dropdown -->
    <div v-if="isAuthenticated" class="relative" @click="toggleDropdown">
      <i class="pi pi-user text-2xl cursor-pointer"></i>
      
      <div v-if="showDropdown" class="absolute right-0 mt-2 w-48 bg-white border rounded shadow-lg z-50">
        <button @click="logout" class="block w-full text-left px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">
          Déconnexion
        </button>
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref } from 'vue';

const showDropdown = ref(false);
const isAuthenticated = ref(true); // Suppose que l'utilisateur est connecté
const pageTitle = ref(''); // On passera la valeur depuis le layout

const toggleDropdown = () => {
  showDropdown.value = !showDropdown.value;
};

const logout = () => {
  console.log('Déconnexion réussie !');
};
</script>
