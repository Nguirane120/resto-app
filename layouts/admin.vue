<template>
  <div class="admin-dashboard h-screen bg-gray-100 flex flex-col md:flex-row">
    <!-- Sidebar -->
    <div
      :class="[
        'fixed inset-y-0 left-0 z-20 bg-white md:static md:flex md:flex-col transition-transform',
        isSidebarOpen || isDesktop ? 'translate-x-0' : '-translate-x-full',
      ]"
    >
      <Sidebar :menuItems="menuItems"/>
    </div>

    <!-- Main Content -->
    <div class="flex-1 flex flex-col overflow-y-auto">
      <!-- Header -->
      <Header :pageTitle="pageTitle" @toggleSidebar="toggleSidebar"/>

      <!-- Slot for main content -->
      <div class="flex-1 p-4">
        <slot />
      </div>
    </div>
    
    <!-- Backdrop -->
    <div 
      v-if="isSidebarOpen && !isDesktop" 
      @click="toggleSidebar" 
      class="fixed inset-0 bg-black bg-opacity-50 z-10"
    ></div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import Header from '~/components/dashboard/header.vue';
import Sidebar from '~/components/dashboard/sidebar-admin.vue';

const menuItems = [
  { path: '/admin/dashboard', label: 'Tableau de Bord', icon: 'pi pi-th-large' },
  { path: '/admin/products', label: 'Produits', icon: 'pi pi-shopping-bag' },
  { path: '/admin/categories', label: 'Catégories', icon: 'pi pi-tags' },
  { path: '/admin/orders', label: 'Commandes', icon: 'pi pi-shopping-cart' },
  { path: '/admin/clients', label: 'Clients', icon: 'pi pi-users' },
  { path: '/admin/settings', label: 'Paramètres', icon: 'pi pi-cog' }
];

const pageTitle = ref('Tableau de Bord');
const isSidebarOpen = ref(false);
const isDesktop = ref(false);

function toggleSidebar() {
  isSidebarOpen.value = !isSidebarOpen.value;
}

function handleResize() {
  isDesktop.value = window.innerWidth >= 768;
  if (isDesktop.value) {
    isSidebarOpen.value = false; // Fermer le sidebar si on est sur desktop
  }
}

onMounted(() => {
  handleResize();
  window.addEventListener('resize', handleResize);
});
</script>
