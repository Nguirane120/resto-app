layouts/admin.vue
<template>
    <div class="admin-dashboard flex h-screen bg-gray-100">
      <!-- Sidebar -->
      <Sidebar :menuItems="menuItems"/>
  
      <!-- Main Content -->
      <div class="flex-1 overflow-y-auto">
      <Header :pageTitle="pageTitle"/>
        <slot />
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import Header from '~/components/dashboard/header.vue';
  import Sidebar from '~/components/dashboard/sidebar-admin.vue';


  
  const currentRoute = ref('/dashboard');
  
  const menuItems = [
    { path: '/admin/dashboard', label: 'Tableau de Bord', icon: 'pi pi-th-large' },
    { path: '/admin/products', label: 'Produits', icon: 'pi pi-shopping-bag' },
    { path: '/admin/categories', label: 'Catégories', icon: 'pi pi-tags' },
    { path: '/admin/orders', label: 'Commandes', icon: 'pi pi-shopping-cart' },
    { path: '/admin/clients', label: 'Clients', icon: 'pi pi-users' },
    { path: '/admin/settings', label: 'Paramètres', icon: 'pi pi-cog' }
  ];

  const pageTitle = ref('Tableau de Bord'); // Par défaut

  </script>
  