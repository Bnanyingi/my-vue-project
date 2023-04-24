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
    
                    <div class="mt-6">
                        <button type="submit" class="px-4 py-2 text-white bg-indigo-500 rounded-md hover:bg-indigo-600" >Update Customer</button>
                        <button type="button" class="px-4 py-2 ml-4 text-gray-700 rounded-md hover:bg-gray-100" @click="closeModal">Cancel</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
    </template>
    
    <script>
    export default {
        props: ['customer'],
        data() {
            return {
                updatedCustomer: {
                    name: this.customer?.name,
                    email: this.customer?.email,
                    phone: this.customer?.phone,
                    label: this.customer?.label,
                    branch: this.customer?.branch
                }
            }
        },
        methods: {
            updateCustomer() {
                if (this.customer) {
                    fetch(`http://localhost:3000/customers/${this.customer.id}`, {
                            method: 'PUT',
                            headers: {
                                'Content-Type': 'application/json'
                            },
                            body: JSON.stringify(this.updatedCustomer)
                        })
                        .then(response => response.json())
                        .then(data => {
    
                            this.$emit('customer-updated', data);
                            this.$forceUpdate();
                        })
                        .catch(error => console.error(error));
                        
    
                    this.closeModal();
                    
    
                }

                
                
    
            },
            closeModal() {
                this.$emit('close');
                 
            },
            
        }
    }
    </script>