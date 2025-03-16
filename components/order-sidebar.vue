<!-- components/OrderSidebar.vue -->
<template>
    <div class="bg-white rounded-lg">
      <h2 class="text-xl font-bold mb-4">Order</h2>
      
      <!-- Liste des articles -->
      <div class="max-h-[calc(100vh-280px)] overflow-y-auto">
        <div v-if="cartItems.length === 0" class="py-8 text-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 mx-auto text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
          </svg>
          <p class="mt-2 text-gray-500">Your cart is empty</p>
          <button class="mt-3 text-sm text-primary font-medium">Browse Menu</button>
        </div>
        
        <div v-else>
          <OrderItem 
            v-for="item in cartItems" 
            :key="item.id"
            :id="item.id"
            :name="item.title || item.name"
            :price="item.price"
            :quantity="item.quantity"
            :toppings="item.toppings"
            :sauces="item.sauces"
            :imgSrc="item.imgSrc"
            @update:quantity="updateQuantity"
            @remove="removeItem"
          />
        </div>
      </div>
      
      <!-- Coupon -->
      <div class="flex justify-between items-center my-4 pt-2">
        <input 
          v-model="couponCode"
          type="text" 
          placeholder="Add coupon" 
          class="p-2 border border-gray-300 rounded flex-1 mr-2 text-sm"
        />
        <button 
          @click="applyCoupon"
          :class="[
            'px-4 py-2 rounded font-medium text-sm',
            couponCode ? 'bg-yellow-400 hover:bg-yellow-500' : 'bg-gray-200 text-gray-400 cursor-not-allowed'
          ]"
          :disabled="!couponCode"
        >
          Claim
        </button>
      </div>
      
      <!-- Résumé de commande -->
      <div class="mt-4 pt-3 border-t border-gray-100">
        <!-- Sous-total -->
        <div class="flex justify-between items-center mb-2">
          <span class="text-sm text-gray-600">Subtotal</span>
          <span class="font-semibold">€{{ subtotal.toFixed(2) }}</span>
        </div>
        
        <!-- Frais de livraison -->
        <div v-if="deliveryFee > 0" class="flex justify-between items-center mb-2">
          <span class="text-sm text-gray-600">Delivery Fee</span>
          <span class="font-medium">€{{ deliveryFee.toFixed(2) }}</span>
        </div>
        
        <!-- Remise coupon -->
        <div v-if="discount > 0" class="flex justify-between items-center mb-2 text-green-600">
          <span class="text-sm">Discount</span>
          <span class="font-medium">-€{{ discount.toFixed(2) }}</span>
        </div>
        
        <!-- Total -->
        <div class="flex justify-between items-center mt-3 pt-3 border-t border-gray-100">
          <span class="font-bold">Total</span>
          <span class="font-bold text-lg">€{{ total.toFixed(2) }}</span>
        </div>
      </div>
      
      <!-- Bouton de paiement -->
      <button 
        @click="checkout"
        :disabled="cartItems.length === 0"
        :class="[
          'w-full py-3 mt-4 rounded-lg font-bold text-white flex items-center justify-center',
          cartItems.length > 0 ? 'bg-primary hover:bg-red-600' : 'bg-gray-300 cursor-not-allowed'
        ]"
      >
        <span>Proceed to checkout</span>
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 ml-2" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd" />
        </svg>
      </button>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
  const couponCode = ref('');
  const appliedCoupon = ref(null);
  const deliveryFee = ref(3); // On met un frais de livraison statique
  
  // Articles du panier (données statiques)
  const cartItems = ref([
    {
      id: 1,
      title: 'Pizza Margherita',
      price: 10,
      quantity: 2,
      toppings: 'Basil',
      sauces: 'Tomato Sauce',
      imgSrc: 'https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg'
    },
    {
      id: 2,
      title: 'Burger Deluxe',
      price: 8,
      quantity: 1,
      toppings: 'Lettuce', 
      sauces: 'Ketchup',
      imgSrc: 'https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg'
    }
  ]);
  
  // Calcul du sous-total
  const subtotal = computed(() => {
    return cartItems.value.reduce((acc, item) => acc + item.price * item.quantity, 0);
  });
  
  // Remise (basée sur le coupon appliqué)
  const discount = computed(() => {
    if (!appliedCoupon.value) return 0;
  
    if (appliedCoupon.value.type === 'percentage') {
      return (subtotal.value * appliedCoupon.value.value) / 100;
    } else if (appliedCoupon.value.type === 'fixed') {
      return appliedCoupon.value.value;
    }
    return 0;
  });
  
  // Calcul du total
  const total = computed(() => {
    return subtotal.value + deliveryFee.value - discount.value;
  });
  
  // Appliquer un coupon
  const applyCoupon = () => {
    if (couponCode.value.toUpperCase() === 'PIZZA20') {
      appliedCoupon.value = { code: 'PIZZA20', type: 'percentage', value: 20 };
      couponCode.value = '';
    } else if (couponCode.value.toUpperCase() === 'FREE5') {
      appliedCoupon.value = { code: 'FREE5', type: 'fixed', value: 5 };
      couponCode.value = '';
    } else {
      alert('Invalid coupon code');
    }
  };
  
  // Checkout
  const checkout = () => {
    alert(`Processing order for €${total.value.toFixed(2)}`);
  };

  const updateQuantity = () =>{

  }

  const removeItem = () =>{

  }
  </script>
  