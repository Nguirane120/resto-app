<template>
    <div class="w-64 bg-gray-800 text-white h-full flex flex-col justify-between overflow-y-auto">
      <!-- Sidebar Header -->
      <div>
        <div class="p-4 border-b border-gray-700">
          <h1 class="text-xl font-bold">Admin</h1>
        </div>
  
        <!-- Navigation Links -->
        <nav class="mt-4">
          <nuxt-link
            v-for="item in menuItems"
            :key="item.path"
            :to="item.path"
            class="block px-4 py-3 hover:bg-gray-700 transition-colors"
            :class="{ 'bg-gray-700': currentRoute === item.path }"
          >
            <div class="flex items-center">
              <i :class="['mr-3', item.icon]"></i>
              <span>{{ item.label }}</span>
            </div>
          </nuxt-link>
        </nav>
      </div>
  
      <!-- Profile Section -->
      <div class="p-4 border-t border-gray-700 flex items-center">
        <img src="" class="rounded-full w-10 h-10 mr-3" alt="Admin" />
        <div>
          <div class="font-semibold">Admin User</div>
          <div class="text-xs text-gray-400">admin@pizzaapp.com</div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  
  const currentRoute = ref('/admin/dashboard');
defineProps({
    menuItems:Array
})
  </script>
  
  <style scoped>
  body {
    margin: 0;
    font-family: sans-serif;
  }
  </style>
  