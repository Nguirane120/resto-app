<!-- components/SearchBar.vue (suite) -->
<template>
    <div class="p-4 sticky top-0 bg-white z-20 shadow-sm">
      <div class="relative">
        <input 
          v-model="searchQuery"
          type="text" 
          placeholder="Search your dish" 
          class="w-full p-3 pl-10 pr-10 rounded-full border border-gray-300 focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent"
        />
        
        <!-- Icône de recherche -->
        <div class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-500">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
        </div>
        
        <!-- Icône de filtre -->
        <div class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-500">
          <button @click="toggleFilters" class="p-1">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 4a1 1 0 011-1h16a1 1 0 011 1v2.586a1 1 0 01-.293.707l-6.414 6.414a1 1 0 00-.293.707V17l-4 4v-6.586a1 1 0 00-.293-.707L3.293 7.293A1 1 0 013 6.586V4z" />
            </svg>
          </button>
        </div>
      </div>
      
      <!-- Zone de filtres (cachée par défaut) -->
      <div v-if="showFilters" class="mt-4 p-4 bg-gray-50 rounded-lg border border-gray-200 animated fadeIn">
        <h3 class="font-medium mb-2">Filtres</h3>
        
        <div class="space-y-2">
          <div>
            <label class="flex items-center space-x-2">
              <input type="checkbox" class="form-checkbox text-primary rounded" v-model="filters.vegetarian">
              <span>Végétarien</span>
            </label>
          </div>
          
          <div>
            <label class="flex items-center space-x-2">
              <input type="checkbox" class="form-checkbox text-primary rounded" v-model="filters.spicy">
              <span>Épicé</span>
            </label>
          </div>
          
          <div>
            <label class="flex items-center space-x-2">
              <input type="checkbox" class="form-checkbox text-primary rounded" v-model="filters.popular">
              <span>Populaire</span>
            </label>
          </div>
        </div>
        
        <div class="mt-3">
          <label class="block text-sm font-medium mb-1">Gamme de prix:</label>
          <div class="flex items-center space-x-2">
            <input 
              type="range" 
              min="0" 
              max="30" 
              step="1"
              v-model.number="filters.maxPrice" 
              class="w-full"
            >
            <span class="text-sm font-medium">€{{ filters.maxPrice }}</span>
          </div>
        </div>
        
        <div class="mt-4 flex justify-end space-x-2">
          <button @click="resetFilters" class="px-3 py-1 border border-gray-300 rounded text-sm">
            Réinitialiser
          </button>
          <button @click="applyFilters" class="px-3 py-1 bg-primary text-white rounded text-sm">
            Appliquer
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, reactive } from 'vue';
  
  const searchQuery = ref('');
  const showFilters = ref(false);
  
  const filters = reactive({
    vegetarian: false,
    spicy: false,
    popular: false,
    maxPrice: 30
  });
  
  const toggleFilters = () => {
    showFilters.value = !showFilters.value;
  };
  
  const resetFilters = () => {
    filters.vegetarian = false;
    filters.spicy = false;
    filters.popular = false;
    filters.maxPrice = 30;
  };
  
  const applyFilters = () => {
    // Ici vous pourriez émettre un événement avec les filtres
    // ou utiliser un store pour appliquer les filtres
    
    // Cacher les filtres après application
    showFilters.value = false;
    
    // Exemple d'émission d'événement
    emit('filter', {
      query: searchQuery.value,
      filters: { ...filters }
    });
  };
  
  const emit = defineEmits(['search', 'filter']);
  
  // Surveiller les changements dans la barre de recherche
  watch(searchQuery, (newValue) => {
    emit('search', newValue);
  });
  </script>
  
  <style scoped>
  .animated {
    animation-duration: 0.3s;
    animation-fill-mode: both;
  }
  
  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(-10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .fadeIn {
    animation-name: fadeIn;
  }
  </style>