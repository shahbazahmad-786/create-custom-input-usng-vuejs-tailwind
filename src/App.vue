<template>
  <div class="w-full h-screen flex justify-center items-center">
    <div class="w-[60%] bg-white rounded-md p-5 shadow-lg">
      <!-- Form -->
      <form @submit.prevent="onSubmit">
        <!-- CustomInput Fields -->
        <CustomInput class="mb-3" v-model="model.title" label="Title" :required="true" />
        <CustomInput type="file" class="mb-3" label="Product Image" @change="file => model.image = file" />
        <CustomInput type="textarea" class="mb-3" v-model="model.description" label="Description" :required="true" />
        <CustomInput type="number" class="mb-3" v-model="model.price" label="Price" prepend="$" :required="true" />
        <CustomInput type="select" class="mb-3" v-model="model.gender" :selectOptions="genderOptions" />
        <CustomInput type="checkbox" class="mb-3" v-model="model.published" label="Published" />
        <!--/ CustomInput Fields -->

        <!-- Buttons -->
        <button type="submit"
          class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 text-base font-medium focus:outline-none focus:ring-2 focus:ring-offset-2 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:ring-indigo-500">
          Submit
        </button>
        <input type="reset" value="Reset"
          class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm" />
        <!--/ Buttons -->
      </form>
      <!--/ Form -->
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue';
import CustomInput from './components/CustomInput.vue';

/* Empty Form-Models */
const model = ref({
  title: '',
  file: null,
  description: '',
  price: null,
  gender: null,
  published: null,
});

/* Gender Options */
const genderOptions = [{ text: 'male' }, { text: 'female' }, { text: 'custom' }];

/* On Form Submitted */
const onSubmit = () => alert('Form Submitted');
</script>
