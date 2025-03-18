<template>
    <header class="bg-white shadow-sm p-4 flex justify-between items-center">
      <h2 class="text-xl font-semibold">{{ pageTitle }}</h2>
      
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
  import { ref, onMounted } from 'vue';
  import { useRouter } from 'vue-router';
  
  const router = useRouter();
  const showDropdown = ref(false);
  const isAuthenticated = ref(true); // Suppose que l'utilisateur est connecté
  const pageTitle = ref(''); // On passera la valeur depuis le layout
  
  const toggleDropdown = () => {
    showDropdown.value = !showDropdown.value;
  };
  
  const checkAuth = () => {
    // const token = localStorage.getItem('token');
    // if (token) {
    //   isAuthenticated.value = true;
    // } else {
    //   router.push('/login');
    // }
  };
  
  const logout = () => {
    // localStorage.removeItem('token');
    // isAuthenticated.value = false;
    // router.push('/login');
  };
  
//   onMounted(() => {
//     checkAuth();
//   });
  </script>
  
  <style scoped>
  body {
    margin: 0;
    font-family: sans-serif;
  }
  </style>
  