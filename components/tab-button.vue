<!-- components/CategoryTabs.vue -->
<template>
    <div class="border-b border-gray-200">
      <div class="flex overflow-x-auto scrollbar-hide p-1">
        <button
          v-for="category in categories"
          :key="category.id"
          @click="setActiveCategory(category.id)"
          :class="[
            'whitespace-nowrap px-4 py-2 mx-1 text-sm font-medium transition-all',
            activeCategory === category.id
              ? 'text-primary border-b-2 border-primary'
              : 'text-gray-500 hover:text-gray-800'
          ]"
        >
          {{ category.name }}
          <span 
            v-if="category.count" 
            :class="[
              'ml-1 px-1.5 py-0.5 text-xs rounded-full', 
              activeCategory === category.id
                ? 'bg-primary text-white'
                : 'bg-gray-200 text-gray-700'
            ]"
          >
            {{ category.count }}
          </span>
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const props = defineProps({
    initialCategory: {
      type: String,
      default: 'classic-pizzas'
    }
  });
  
  const emit = defineEmits(['category-change']);
  
  const categories = [
    { id: 'classic-pizzas', name: 'Classic Pizzas', count: '8' },
    { id: 'starters', name: 'Starters', count: '7' },
    { id: 'desserts', name: 'Desserts', count: '3' },
    { id: 'drinks', name: 'Drinks', count: '4' },
    { id: 'canned-beverages', name: 'Canned Beverages', count: '3' },
    { id: 'extras', name: 'Extras', count: null }
  ];
  
  const activeCategory = ref(props.initialCategory);
  
  const setActiveCategory = (categoryId) => {
    activeCategory.value = categoryId;
    emit('category-change', categoryId);
  };
  </script>
  
  <style scoped>
  /* Masquer la barre de défilement tout en préservant la fonctionnalité */
  .scrollbar-hide {
    -ms-overflow-style: none;  /* IE et Edge */
    scrollbar-width: none;  /* Firefox */
  }
  
  .scrollbar-hide::-webkit-scrollbar {
    display: none;  /* Chrome, Safari et Opera */
  }
  </style>