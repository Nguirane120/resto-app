<template>
  <div class="min-h-screen bg-gray-100">
    <!-- Header avec promo -->
    <Header/>
    
    <!-- Search bar -->
    <SearchBar/>
    
    <!-- Navigation tabs -->
    <div class="flex overflow-x-auto px-4 border-b">
      <TabButton />
    </div>
    
    <!-- Pizza grid -->
    <div class="p-4">
      <h2 class="text-lg font-semibold mb-4">Classic Pizzas</h2>
      <div class="grid grid-cols-2 gap-4">
        <Card 
          v-for="(pizza, index) in pizzas" 
          :key="index"
          :title="pizza.title" 
          :price="pizza.price" 
          :imgSrc="pizza.imgSrc"
          :isPopular="pizza.isPopular"
          :isHighlyRated="pizza.isHighlyRated"
          @click="openPizzaDetails(pizza)"
        />
      </div>
    </div>
    
    <!--  Detail Modal -->
    <!-- <DetailProduct
      v-model:visible="showPizzaModal"
      :pizza="selectedPizza"
      :toppings="toppings"
      @add-to-cart="handleAddToCart"
    /> -->
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { TabButton, Card, Header, SearchBar } from '#components';
import DetailProduct from '~/components/detail-product.vue';

// Données des pizzas
const pizzas = ref([
  {
    id: 'capricciosa',
    title: "Capricciosa", 
    price: "€10.50", 
    imgSrc: "https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg",
    isPopular: true,
    description: "Ham, Mushroom, Olives & Mozzarella on a Tomato base (Anchovies optional)"
  },
  {
    id: 'pesto-chicken',
    title: "Pesto Chicken Club", 
    price: "€11.50", 
    imgSrc: "https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg",
    isHighlyRated: true,
    description: "Grilled chicken, pesto, fresh tomatoes & mozzarella cheese"
  },
  {
    id: 'pesto-veg',
    title: "Pesto Veggie", 
    price: "€12.50", 
    imgSrc: "https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg",
    description: "Pesto base with roasted vegetables, mushrooms & spinach"
  },
  {
    id: 'bbq-chicken',
    title: "BBQ Chicken", 
    price: "€13.50", 
    imgSrc: "https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg",
    description: "BBQ sauce, grilled chicken, red onions & mozzarella"
  }
]);

// Données des toppings
const toppings = ref([
  { name: 'Pepperoni', price: '1.50', icon: 'pi pi-circle-fill text-red-500' },
  { name: 'Sausages', price: '1.50', icon: 'pi pi-stop-circle text-yellow-700' },
  { name: 'Grilled Chicken', price: '2.00', icon: 'pi pi-circle text-orange-300' },
  { name: 'Bacon', price: '2.00', icon: 'pi pi-bars text-red-300' }
]);

// État du modal
const showPizzaModal = ref(false);
const selectedPizza = ref({});

// Ouvrir le modal avec les détails de la pizza
const openPizzaDetails = (pizza) => {
  selectedPizza.value = pizza;
  showPizzaModal.value = true;
  
};

// Gérer l'ajout au panier
const handleAddToCart = (item) => {
  console.log('Added to cart:', item);
  // Implémentez votre logique d'ajout au panier ici
  // Par exemple, stockez dans un state global ou localStorage
};
</script>
