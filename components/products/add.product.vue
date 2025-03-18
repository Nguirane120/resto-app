<template>
  <Dialog 
    v-model:visible="dialogVisible" 
    :modal="true" 
    :closable="false"
    @update:visible="updateVisibility"
    :style="{ width: '50vw' }" :breakpoints="{ '1199px': '75vw', '575px': '90vw' }" :headerClass="'bg-white'">

    <div class="p-4 bg-white rounded-xl shadow-lg">
      <div class="flex  items-center justify-between mb-5">
        <h1 class="text-xl font-semibold">Ajouter un produit</h1>
        <button class="cursor-pointer" @click="dialogVisible = false">
          <i class="pi pi-times"></i>
        </button>
      </div>

      <CustumInput 
        id="title"
        label="Titre"
        v-model="productData.title"
        :isInvalid="submitted && !productData.title"
        errorMessage="Le titre est requis."
      />

      <CustumInput 
        id="price"
        label="Prix"
        v-model="productData.price"
        :isInvalid="submitted && !productData.price"
        errorMessage="Le prix est requis."
      />

      <CustumInput 
        id="category"
        label="Catégorie"
        v-model="productData.category"
        type="select"
        :options="['Entrée', 'Plat Principal', 'Dessert', 'Boisson', 'Snack']"
        :isInvalid="submitted && !productData.category"
        errorMessage="La catégorie est requise."
      />

      <CustumInput 
        id="imgSrc"
        label="Image"
        v-model="productData.imgSrc"
        type="file"
        @change="onFileChange"
        :isInvalid="submitted && !productData.imgSrc"
        errorMessage="L'image est requise."
      />

      <div class="flex justify-end mt-6 space-x-2">
        <button @click="cancel" class="px-4 py-2 text-gray-700 bg-gray-200 rounded-lg hover:bg-gray-300">Annuler</button>
        <button @click="submitProduct" class="px-4 py-2 text-white bg-blue-600 rounded-lg hover:bg-blue-700">
          {{ isEditMode ? 'Modifier' : 'Ajouter' }}
        </button>
      </div>
    </div>
  </Dialog>
</template>

<script setup>
import { ref, reactive, computed, watch, onMounted } from 'vue';
import Dialog from 'primevue/dialog';
import { CustumInput } from '#components';

const props = defineProps({
  visible: Boolean,
  product: Object,
  isEdit: Boolean
});

const emit = defineEmits(['update:visible', 'save-product', 'cancel']);

const dialogVisible = computed({
  get: () => props.visible,
  set: (value) => emit('update:visible', value)
});

const isEditMode = computed(() => props.isEdit);
const submitted = ref(false);
const productData = reactive({
  title: '',
  price: '',
  category: '',
  imgSrc: null
});

const onFileChange = (event) => {
  const file = event.target.files[0];
  productData.imgSrc = file ? file : null;
};

const submitProduct = () => {
  submitted.value = true;
  if (validateForm()) {
    const productToSave = { ...productData };
    emit('save-product', productToSave);
    dialogVisible.value = false;
    submitted.value = false;
  }
};

const validateForm = () => {
  return productData.title && productData.price && productData.imgSrc && productData.category;
};

const cancel = () => {
  emit('cancel');
  dialogVisible.value = false;
  submitted.value = false;
};

const updateVisibility = (value) => {
  dialogVisible.value = value;
};
</script>

<style scoped>
.p-dialog .p-fluid .field {
  margin-bottom: 1rem;
}

.p-dialog .p-fluid .field label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
}

.p-error {
  color: #f44336;
  font-size: 0.75rem;
  margin-top: 0.25rem;
}

.p-invalid {
  border-color: #f44336 !important;
}
</style>
