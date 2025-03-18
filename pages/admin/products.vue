<template>
    <div>
        <div class="flex justify-between py-5">
           <h1>Liste des produit</h1>
           <buttton class="bg-black text-white border-1 p-3 rounded-md cursor-pointer" @click="openDialog">Ajouter</buttton>
        </div>
      <ProductList/>
      <AddProduct v-if="dialogVisible"  v-model:visible="dialogVisible"
      :product="selectedProduct"
      :isEdit="isEditMode"
      @save-product="saveProduct"
      @cancel="handleCancel"/>
    </div>
  </template>
  
  <script setup>
  import ProductList from '~/components/products/product-list.vue';
  import AddProduct from '~/components/products/add.product.vue';
  
  definePageMeta({
    layout: 'admin'
  });


  const dialogVisible = ref(false);
const isEditMode = ref(false);
const selectedProduct = ref({});
const openDialog = () => {
  isEditMode.value = false;
  selectedProduct.value = {
    title: '',
    price: '',
    originalPrice: '',
    imgSrc: '',
    isPopular: false,
    isHighlyRated: false,
    badgeNumber: ''
  };
  dialogVisible.value = true;
};

const saveProduct = (product) => {
  // Ici, vous pouvez appeler une API ou émettre un événement vers ProductList
  console.log('Produit sauvegardé :', product);
  
  // Exemple : Si ProductList a une méthode pour actualiser les données
  // Vous pourriez avoir une référence à ProductList avec ref()
  // productListRef.value.refreshData();
  
  dialogVisible.value = false;
};
const editProduct = (product) => {
  isEditMode.value = true;
  selectedProduct.value = { ...product };
  dialogVisible.value = true;
};
// Gérer l'annulation
const handleCancel = () => {
  dialogVisible.value = false;
};


  </script>
  