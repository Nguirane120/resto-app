<template>
  <Dialog 
    :visible="visible" 
    :style="{ width: '90vw', maxWidth: '500px' }" 
    :modal="true" 
    :closable="true"
    class=""
  >
  <div class="flex flex-col space-y-4 bg-white">
      <!-- Pizza Image -->
      <div class="relative">
        <img :src="img" alt="Pizza" class="w-full h-64 object-cover rounded-lg shadow" />
      </div>

      <!-- Pizza Information -->
      <div class="space-y-2">
        <h2 class="text-xl font-bold text-gray-800">{{ pizza.name }}</h2>
        <p class="text-gray-600">{{ pizza.description }}</p>
      </div>

      <!-- Size Selection -->
      <div class="flex space-x-4 items-center">
        <span class="font-medium">Size:</span>
        <Button 
          v-for="size in ['Regular', 'Large']" 
          :key="size" 
          :label="size" 
          :class="['p-button-sm', selectedSize === size ? 'p-button-primary' : 'p-button-outlined']"
        
        />
      </div>

      <!-- Toppings Selection -->
      <div>
        <span class="font-medium">Toppings:</span>
        <div class="grid grid-cols-3 gap-2 mt-2">
          <Button 
            v-for="topping in toppings" 
            :key="topping.name"
            :label="topping.name" 
            :class="['p-button-sm', selectedToppings.includes(topping) ? 'p-button-primary' : 'p-button-outlined']"
            @click="toggleTopping(topping)"
          />
        </div>
      </div>

      <!-- Quantity Selector and Total Price -->
      <div class="flex items-center justify-between">
        <div class="flex items-center space-x-2">
          <Button icon="pi pi-minus" class="p-button-sm" @click="decreaseQuantity" />
          <span class="font-bold text-lg">{{ quantity }}</span>
          <Button icon="pi pi-plus" class="p-button-sm" @click="increaseQuantity" />
        </div>
        <div class="text-lg font-bold">Total: {{ calculateTotalPrice() }} USD</div>
      </div>

      <!-- Add to Cart Button -->
      <Button label="Add to Cart" class="p-button-success w-full p-button-lg mt-4" @click="addToCart" />
    </div>
  </Dialog>
</template>

<script setup>
import { Button, Dialog } from 'primevue';
import { ref, computed, watch } from 'vue';


const img = ref("https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg",)

const props = defineProps({
  visible: {
    type: Boolean,
    required: true
  },
  pizza: {
    type: Object,
    required: true,
    default: () => ({
      title: '',
      imgSrc: '',
      price: '0',
      description: ''
    })
  },
  toppings: {
    type: Array,
    default: () => [
      { name: 'Pepperoni', price: '1.50', icon: 'pi pi-circle-fill text-red-500' },
      { name: 'Sausages', price: '1.50', icon: 'pi pi-stop-circle text-yellow-700' },
      { name: 'Grilled Chicken', price: '2.00', icon: 'pi pi-circle text-orange-300' },
      { name: 'Bacon', price: '2.00', icon: 'pi pi-bars text-red-300' }
    ]
  }
});

const emit = defineEmits(['update:visible', 'add-to-cart']);

const selectedSize = ref('regular');
const quantity = ref(1);
const selectedToppings = ref([]);

// Extraire le prix de base du string (par exemple: "€10.50" -> 10.5)
const basePrice = computed(() => {
  const priceStr = props.pizza.price || '0';
  return parseFloat(priceStr.replace('€', '').trim());
});

const regularPrice = computed(() => basePrice.value.toFixed(2));
const largePrice = computed(() => (basePrice.value * 1.8).toFixed(2)); // Large est 80% plus cher

// Réinitialiser les valeurs quand le modal s'ouvre
watch(() => props.visible, (newValue) => {
  if (newValue) {
    selectedSize.value = 'regular';
    quantity.value = 1;
    selectedToppings.value = [];
  }
});

const decrementQuantity = () => {
  if (quantity.value > 1) {
    quantity.value--;
  }
};

const incrementQuantity = () => {
  quantity.value++;
};

const toggleTopping = (toppingName) => {
  if (selectedToppings.value.includes(toppingName)) {
    selectedToppings.value = selectedToppings.value.filter(t => t !== toppingName);
  } else {
    selectedToppings.value.push(toppingName);
  }
};

const calculateTotalPrice = () => {
  // Prix de base selon la taille
  let total = selectedSize.value === 'regular' ? basePrice.value : basePrice.value * 1.8;
  
  // Ajouter le prix des toppings
  selectedToppings.value.forEach(toppingName => {
    const topping = props.toppings.find(t => t.name === toppingName);
    if (topping) {
      total += parseFloat(topping.price);
    }
  });
  
  // Multiplier par la quantité
  total *= quantity.value;
  
  return total.toFixed(2);
};

const addToCart = () => {
  const cartItem = {
    id: props.pizza.id || props.pizza.title,
    title: props.pizza.title,
    price: selectedSize.value === 'regular' ? regularPrice.value : largePrice.value,
    size: selectedSize.value,
    quantity: quantity.value,
    toppings: selectedToppings.value,
    totalPrice: calculateTotalPrice(),
    imgSrc: props.pizza.imgSrc
  };
  
  emit('add-to-cart', cartItem);
  emit('update:visible', false);
};
</script>

<style scoped>
.pizza-detail-modal :deep(.p-dialog-header) {
  border-bottom: 1px solid #f0f0f0;
}

.pizza-detail-modal :deep(.p-dialog-footer) {
  border-top: 1px solid #f0f0f0;
  padding: 1rem;
}
</style>