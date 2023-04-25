<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  props: {
    customer: {
      type: Object as () => {
        id: number;
        name: string;
        email: string;
        phone: string;
        label: string;
        branch: string;
      },
      required: true,
    },
  },
  setup(props, { emit }) {
    const updatedCustomer = ref({
      name: props.customer?.name,
      email: props.customer?.email,
      phone: props.customer?.phone,
      label: props.customer?.label,
      branch: props.customer?.branch,
      salesRep: props.customer?.salesRep,
    });

    const updateCustomer = (): void => {
      if (props.customer) {
        fetch(`http://localhost:3000/customers/${props.customer.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(updatedCustomer.value),
        })
          .then(response => response.json())
          .then(data => {
            emit('customer-updated', data);
          })
          .catch(error => console.error(error));

        closeModal();
      }
    };

    const closeModal = (): void => {
      emit('close');
    };

    return {
      updatedCustomer,
      updateCustomer,
      closeModal,
    };
  },
});
</script>

<template>
    <div class="fixed inset-0 z-10 overflow-y-auto">
        <div class="flex items-center justify-center min-h-screen">
            <div class="fixed inset-0 transition-opacity" aria-hidden="true" @click="closeModal"></div>
    
            <div class="overflow-hidden transition-all transform bg-white rounded-lg shadow-xl sm:max-w-lg sm:w-full">
                <form class="p-6" @submit.prevent="updateCustomer">
                    <div class="mb-4">
                        <label for="name" class="block mb-2 font-medium text-gray-700">Name</label>
                        <input type="text" id="name" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="updatedCustomer.name" required>
                    </div>
    
                    <div class="mb-4">
                        <label for="phone" class="block mb-2 font-medium text-gray-700">Phone</label>
                        <input type="phone" id="phone" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="updatedCustomer.phone" required>
                    </div>
    
                    <div class="mb-4">
                        <label for="email" class="block mb-2 font-medium text-gray-700">Email</label>
                        <input type="email" id="email" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="updatedCustomer.email" required>
                    </div>
    
                    <div class="mb-4">
                        <label for="label" class="block mb-2 font-medium text-gray-700">Label</label>
                        <input type="text" id="label" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="updatedCustomer.label" required>
                    </div>
    
                    <div class="mb-4">
                        <label for="branch" class="block mb-2 font-medium text-gray-700">Branch</label>
                        <input type="text" id="branch" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="updatedCustomer.branch" required>
                    </div>

                     <div class="mb-4">
                        <label for="branch" class="block mb-2 font-medium text-gray-700">salesRep</label>
                        <input type="text" id="branch" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="updatedCustomer.salesRep" required>
                    </div>
    
                    <div class="mt-6">
                        <button type="submit" class="px-4 py-2 text-white bg-indigo-500 rounded-md hover:bg-indigo-600" >Update Customer</button>
                        <button type="button" class="px-4 py-2 ml-4 text-gray-700 rounded-md hover:bg-gray-100" @click="closeModal">Cancel</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
    </template>
    
  