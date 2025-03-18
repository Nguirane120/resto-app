<template>
    <div class="mb-4">
      <label :for="id" class="block text-gray-700 font-medium mb-2">{{ label }}</label>
      <input
        :id="id"
        :type="type"
        @input="updateValue($event.target.value)"
        :class="[
          'w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 transition',
          isInvalid ? 'border-red-500' : 'border-gray-300'
        ]"
      />
      <p v-if="isInvalid" class="text-red-500 text-sm mt-1">{{ errorMessage }}</p>
    </div>
  </template>
  
  <script setup>
  
  const props = defineProps({
    modelValue: String,
    label: String,
    id: String,
    type: { type: String, default: 'text' },
    isInvalid: Boolean,
    errorMessage: String,
  });
  
  const emit = defineEmits(['update:modelValue']);
  
  const updateValue = (value) => {
    emit('update:modelValue', value);
  };
  </script>
  