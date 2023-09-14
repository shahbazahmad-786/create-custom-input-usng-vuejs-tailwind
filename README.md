# Create Custom Input Using VueJs and Tailwind

## Project Setup

Create or install the `vue-js` application and `tailwind-css` framework.

## Custom Input Component

Create a component in components dir like `src/components/CustomInput.vue`

#### Vue Template

```html
<template>
  <div>
    <label class="sr-only"> {{ label }} </label>
    <div class="mt-1 flex rounded-md shadow-sm">
      <span
        v-if="prepend"
        class="inline-flex items-center px-3 rounded-l-md border border-r-0 border-gray-300 bg-gray-50 text-gray-500 text-sm"
      >
        {{ prepend }}
      </span>
      <template v-if="type === 'select'">
        <select
          :name="name"
          :required="required"
          :value="props.modelValue"
          :class="inputClasses"
          @change="onChange($event.target.value)"
        >
          <option v-for="option of selectOptions" :value="option.key">{{ option.text }}</option>
        </select>
      </template>
      <template v-else-if="type === 'textarea'">
        <textarea
          :name="name"
          :required="required"
          :value="props.modelValue"
          @input="emit('update:modelValue', $event.target.value)"
          :class="inputClasses"
          :placeholder="label"
        >
        </textarea>
      </template>
      <template v-else-if="type === 'file'">
        <input
          :type="type"
          :name="name"
          :required="required"
          :value="props.modelValue"
          @input="emit('change', $event.target.files[0])"
          :class="inputClasses"
          :placeholder="label"
        />
      </template>
      <template v-else-if="type === 'checkbox'">
        <input
          :id="id"
          :name="name"
          :type="type"
          :checked="props.modelValue"
          :required="required"
          @change="emit('update:modelValue', $event.target.checked)"
          class="h-4 w-4 text-indigo-600 focus:ring-indigo-500 border-gray-300 rounded"
        />
        <label :for="id" class="ml-2 block text-sm text-gray-900"> {{ label }} </label>
      </template>
      <template v-else>
        <input
          :type="type"
          :name="name"
          :required="required"
          :value="props.modelValue"
          @input="emit('update:modelValue', $event.target.value)"
          :class="inputClasses"
          :placeholder="label"
          step="0.01"
        />
      </template>
      <span
        v-if="append"
        class="inline-flex items-center px-3 rounded-r-md border border-l-0 border-gray-300 bg-gray-50 text-gray-500 text-sm"
      >
        {{ append }}
      </span>
    </div>
  </div>
</template>
```

#### Script Setup

```js
<script setup>

import { computed, ref } from "vue";

const props = defineProps({
    modelValue: [String, Number, File],
    label: String,
    type: {
        type: String,
        default: 'text'
    },
    name: String,
    required: Boolean,
    prepend: {
        type: String,
        default: ''
    },
    append: {
        type: String,
        default: ''
    },
    selectOptions: Array
})

const id = computed(() => {
    if (props.id) return props.id;

    return `id-${Math.floor(1000000 + Math.random() * 1000000)}`;
})
const inputClasses = computed(() => {
    const cls = [
        `block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm`,
    ];

    if (props.append && !props.prepend) {
        cls.push(`rounded-l-md`)
    } else if (props.prepend && !props.append) {
        cls.push(`rounded-r-md`)
    } else if (!props.prepend && !props.append) {
        cls.push('rounded-md')
    }
    return cls.join(' ')
})
const emit = defineEmits(['update:modelValue', 'change'])

const onChange = (value) => {
    emit('update:modelValue', value)
    emit('change', value)
}

</script>
```

## Use in project

Import a component `CustomInput.vue` in usage file

#### Vue Template

```html
<template>
  <div class="w-full h-screen flex justify-center items-center">
    <div class="w-[60%] bg-white rounded-md p-5 shadow-lg">
      <!-- Form -->
      <form @submit.prevent="onSubmit">
        <!-- CustomInput Fields -->
        <CustomInput class="mb-3" v-model="model.title" label="Title" :required="true" />
        <CustomInput
          type="file"
          class="mb-3"
          label="Product Image"
          @change="file => model.image = file"
        />
        <CustomInput
          type="textarea"
          class="mb-3"
          v-model="model.description"
          label="Description"
          :required="true"
        />
        <CustomInput
          type="number"
          class="mb-3"
          v-model="model.price"
          label="Price"
          prepend="$"
          :required="true"
        />
        <CustomInput
          type="select"
          class="mb-3"
          v-model="model.gender"
          :selectOptions="genderOptions"
        />
        <CustomInput type="checkbox" class="mb-3" v-model="model.published" label="Published" />
        <!-- CustomInput Fields -->

        <!-- Buttons -->
        <button
          type="submit"
          class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 text-base font-medium focus:outline-none focus:ring-2 focus:ring-offset-2 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:ring-indigo-500"
        >
          Submit
        </button>
        <input
          type="reset"
          value="Reset"
          class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm"
        />
        <!--/ Buttons -->
      </form>
      <!--/ Form -->
    </div>
  </div>
</template>
```

```js
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
```

<div align="center">
&copy; Shahbaz Ahmad | A fullstack web application developer | shahbazahmad0987654321@gmail.com
</div>

<p align="center">
<a href="https://linkedin.com/in/shahbazahmad_sa786" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="shahbazahmad_sa786/" height="20" width="30" /></a>
<a href="https://instagram.com/shahbazahmad_786" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/instagram.svg" alt="shahbazahmad_786" height="20" width="30" /></a>
</p>
