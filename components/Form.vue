<script setup>

const props = defineProps(["data", "handleSubmit"]);
const formData = props.data.input;
const formValues = ref({});

const submitForm = () => {
  props.handleSubmit(formValues.value);
};
</script>

<template>
    <div class="flex items-center justify-center min-h-screen">
      <form @submit.prevent="submitForm" class="border-2 border-sky-200 text-black p-6 rounded-md shadow-md max-w-md mx-auto">
        <div v-for="data in formData" :key="data.name" class="mb-4">
          <label :for="data.name" class="block text-sm font-medium text-gray-700">{{ data.name }}</label>
  
          <template v-if="data.type === 'select'">
            <select
              v-model="formValues[data.name]"
              :name="data.name"
              :id="data.name"
              class="mt-1 block w-full h-10 px-3 border rounded-md focus:outline-none focus:ring-2 focus:ring-sky-400"
              required
            >
              <option v-for="(option, index) in data.options" :key="index">{{ option }}</option>
            </select>
          </template>
  
          <template v-else>
            <input
              v-model="formValues[data.name]"
              :type="data.type"
              :name="data.name"
              :id="data.name"
              :placeholder="data.name"
              class="mt-1 block w-full h-10 px-3 border rounded-md focus:outline-none focus:ring-2 focus:ring-sky-400"
              required
            />
          </template>
        </div>
  
        <button  class="bg-sky-700 p-2 rounded-md text-white">Submit</button>
      </form>
    </div>
  </template>
  
  <style scoped>
    form {
      max-width: 20rem;
      width: 100%;
    }
  </style>