<!-- pages/index.vue -->
<template>
    <div class="min-h-screen bg-gray-100">
      <!-- Header avec promo -->
     <Header/>
      
      <!-- Search bar -->
      
      <SearchBar/>
      <!-- Navigation tabs -->
      <div class="flex overflow-x-auto px-4 border-b">
        <TabButton  />

      </div>
      
      <!-- Pizza grid -->
      <div class="p-4">
        <h2 class="text-lg font-semibold mb-4">Classic Pizzas</h2>
        <div class="grid grid-cols-2 gap-4">
          <Card 
            title="Capricciosa" 
            price="€10.50" 
            imgSrc="https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg"
            isPopular
          />
          <Card 
            title="Pesto Chicken Club" 
            price="€11.50" 
            imgSrc="https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg"
            isHighlyRated
          />
          <Card 
            title="Pesto Chicken Club" 
            price="€12.50" 
            imgSrc="https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg"
          />
          <Card 
            title="BBQ Chicken" 
            price="€13.50" 
            imgSrc="https://img.freepik.com/free-photo/top-view-pepperoni-pizza-with-mushroom-sausages-bell-pepper-olive-corn-black-wooden_141793-2158.jpg"
          />
        </div>
      </div>
      
      <!-- Order sidebar component will be defined separately -->
    </div>
  </template>
  
  <script setup>
  import { TabButton, Card, Header, SearchBar } from '#components'
  
  // Component imports would go here
  </script>