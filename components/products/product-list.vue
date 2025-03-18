<template>
    <div class="card">
      <DataTable 
        :value="products" 
        tableStyle="min-width: 50rem"
        stripedRows
        :paginator="true" 
        :rows="5"
        removableSort
        class="p-datatable-rounded"
      >

  
        <!-- Colonnes du tableau -->
        <Column field="name" header="Name" sortable></Column>
        
        <Column header="Image">
          <template #body="slotProps">
            <img 
              :src="slotProps.data.image || 'https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg'" 
              :alt="slotProps.data.name" 
              class="w-24 h-24 rounded shadow-md object-cover border border-gray-200" 
            />
          </template>
        </Column>
        
        <Column field="price" header="Price" sortable>
          <template #body="slotProps">
            <span class="font-semibold text-primary-700">{{ formatCurrency(slotProps.data.price) }}</span>
          </template>
        </Column>
        
        <Column field="category" header="Category" sortable>
          <template #body="slotProps">
            <Tag :value="slotProps.data.category" :severity="getCategorySeverity(slotProps.data.category)" />
          </template>
        </Column>
        
        <Column header="Actions" class="w-24">
          <template #body="slotProps">
            <div class="flex gap-2">
              <Button 
                icon="pi pi-pencil" 
                class="p-button-rounded p-button-warning p-button-sm"
                @click="editProduct(slotProps.data)"
              />
              <Button 
                icon="pi pi-trash" 
                class="p-button-rounded p-button-danger p-button-sm"
                @click="confirmDelete(slotProps.data)"
              />
            </div>
          </template>
        </Column>
        
        <!-- Footer du tableau -->
        <template #footer>
          <div class="text-sm text-gray-600 italic">
            In total there are {{ products.length }} products.
          </div>
        </template>
  
        <!-- Template pour les états vides -->
        <template #empty>
          <div class="flex flex-col items-center justify-center p-6">
            <i class="pi pi-inbox text-6xl text-gray-300 mb-4"></i>
            <p class="text-gray-500">Aucun produit trouvé.</p>
          </div>
        </template>
      </DataTable>
  

    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  import { DataTable, Column, Dialog, Button, InputText, InputNumber, Dropdown, Tag,  } from 'primevue';
  
  const products = ref([
    { id: 1, name: 'Pizza Margherita', price: 12.99, image: '', category: 'Pizza' },
    { id: 2, name: 'Pizza Pepperoni', price: 14.99, image: '', category: 'Pizza' },
    { id: 3, name: 'Burger Classic', price: 9.99, image: '', category: 'Burger' },
    { id: 4, name: 'Salade César', price: 8.99, image: '', category: 'Salade' },
    { id: 5, name: 'Pasta Carbonara', price: 13.99, image: '', category: 'Pasta' },
  ]);
  
  const showModal = ref(false);
  const isEditing = ref(false);
  const editingId = ref(null);
//   const confirm = useConfirm();
  
  const categories = [
    { name: 'Pizza' },
    { name: 'Burger' },
    { name: 'Pasta' },
    { name: 'Salade' },
    { name: 'Dessert' },
    { name: 'Boisson' }
  ];
  
  const newProduct = ref({
    name: '',
    price: '',
    image: '',
    category: ''
  });
  
  const addProduct = () => {
    const id = Math.max(0, ...products.value.map(p => p.id)) + 1;
    products.value.push({ id, ...newProduct.value });
    resetForm();
  };
  
  const editProduct = (product) => {
    isEditing.value = true;
    editingId.value = product.id;
    newProduct.value = { ...product };
    showModal.value = true;
  };
  
  const updateProduct = () => {
    const index = products.value.findIndex(p => p.id === editingId.value);
    if (index !== -1) {
      products.value[index] = { ...newProduct.value, id: editingId.value };
    }
    resetForm();
  };
  
  const resetForm = () => {
    newProduct.value = { name: '', price: '', image: '', category: '' };
    showModal.value = false;
    isEditing.value = false;
    editingId.value = null;
  };
  
  const confirmDelete = (product) => {
    confirm.require({
      message: `Êtes-vous sûr de vouloir supprimer "${product.name}" ?`,
      header: 'Confirmation de suppression',
      icon: 'pi pi-exclamation-triangle',
      acceptClass: 'p-button-danger',
      accept: () => deleteProduct(product),
    });
  };
  
  const deleteProduct = (product) => {
    products.value = products.value.filter(p => p.id !== product.id);
  };
  
  const formatCurrency = (value) => {
    return parseFloat(value).toLocaleString('en-US', { style: 'currency', currency: 'USD' });
  };
  
  const getCategorySeverity = (category) => {
    switch (category) {
      case 'Pizza':
        return 'success';
      case 'Burger':
        return 'warning';
      case 'Pasta':
        return 'info';
      case 'Salade':
        return 'success';
      case 'Dessert':
        return 'danger';
      case 'Boisson':
        return 'info';
      default:
        return null;
    }
  };
  </script>
  
  <style scoped>
  .p-datatable-rounded {
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  }
  
  .p-datatable-rounded .p-datatable-header {
    background-color: #f8f9fa;
    padding: 1rem;
    border-bottom: 1px solid #e9ecef;
  }
  
  .p-datatable-rounded .p-datatable-footer {
    background-color: #f8f9fa;
    padding: 0.75rem 1rem;
    border-top: 1px solid #e9ecef;
  }
  
  /* Styliser les lignes du tableau */
  :deep(.p-datatable-tbody > tr) {
    transition: background-color 0.2s;
  }
  
  :deep(.p-datatable-tbody > tr:hover) {
    background-color: #f0f7ff !important;
  }
  
  /* Styliser les en-têtes de colonne */
  :deep(.p-column-header-content) {
    font-weight: 600;
  }
  
  /* Effet de survol et focus sur les boutons */
  :deep(.p-button:hover) {
    transform: translateY(-1px);
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  }
  
  :deep(.p-button:active) {
    transform: translateY(0);
  }
  </style>