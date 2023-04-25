<script lang="ts">
import {
    defineComponent,
    ref
} from 'vue';

export default defineComponent({
    name: 'AddCustomerModal',
    props: {
        close: Function,
        customerAdded: {
            type: Function as PropType < (customer: Customer) => void > ,
            required: true,
        },
    },
    setup(props, {
        emit
    }) {
        const customer = ref({
            name: '',
            email: '',
            phone: '',
            label: '',
            branch: '',
            salesRep: '',
        });

        const addCustomer = async () => {
            try {
                const response = await fetch('http://localhost:3000/customers', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(customer.value),
                });
                if (response.status === 201) {
                    const data = await response.json();
                    props.customerAdded(data);
                    closeModal();
                } else {
                    console.error('Failed to add customer.');
                }
            } catch (error) {
                console.error(error);
            }
        };

        const closeModal = (): void => {
            emit('close');
        };

        return {
            customer,
            addCustomer,
            closeModal
        };
    },
});
</script>

<template>
<div class="fixed inset-0 z-10 overflow-y-auto">
    <div class="flex items-center justify-center min-h-screen">
        <div class="fixed inset-0 transition-opacity" aria-hidden="true" @click="closeModal"></div>

        <div class="overflow-hidden transition-all transform bg-white rounded-lg shadow-xl sm:max-w-lg sm:w-full">
            <form class="p-6" @submit.prevent="addCustomer">
                <div class="mb-4">
                    <label for="name" class="block mb-2 font-medium text-gray-700">Name</label>
                    <input type="text" id="name" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="customer.name" required>
                </div>

                <div class="mb-4">
                    <label for="phone" class="block mb-2 font-medium text-gray-700">Phone</label>
                    <input type="phone" id="phone" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="customer.phone" required>
                </div>

                <div class="mb-4">
                    <label for="email" class="block mb-2 font-medium text-gray-700">Email</label>
                    <input type="email" id="email" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="customer.email" required>
                </div>

                <div class="mb-4">
                    <label for="labell" class="block mb-2 font-medium text-gray-700">Label</label>
                    <input type="labell" id="labell" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="customer.label" required>
                </div>

                <div class="mb-4">
                    <label for="branch" class="block mb-2 font-medium text-gray-700">Branch</label>
                    <input type="branch" id="branch" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="customer.branch" required>
                </div>

                  <div class="mb-4">
                    <label for="branch" class="block mb-2 font-medium text-gray-700">SalesRep</label>
                    <input type="branch" id="branch" class="w-full p-2 border rounded outline-none ring-indigo-300 focus:ring form-input" v-model="customer.salesRep" required>
                </div>

                <div class="mt-6">
                    <button type="submit" class="px-4 py-2 text-white bg-indigo-500 rounded-md hover:bg-indigo-600">Add Customer</button>
                    <button type="button" class="px-4 py-2 ml-4 text-gray-700 rounded-md hover:bg-gray-100" @click="closeModal">Cancel</button>
                </div>
            </form>
        </div>
    </div>
</div>
</template>
