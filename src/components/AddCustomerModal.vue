<template>
  <div class="fixed inset-0 z-10 overflow-y-auto">
    <div class="flex items-center justify-center min-h-screen">
      <div class="fixed inset-0 transition-opacity" aria-hidden="true" @click="closeModal"></div>

      <div class="overflow-hidden transition-all transform bg-white rounded-lg shadow-xl sm:max-w-lg sm:w-full">
        <form class="p-6" @submit.prevent="addCustomer">
          <div class="mb-4">
            <label for="name" class="block mb-2 font-medium text-gray-700">Name</label>
            <input type="text" id="name" class="w-full form-input" v-model="customer.name" required>
          </div>

          <div class="mb-4">
            <label for="email" class="block mb-2 font-medium text-gray-700">Email</label>
            <input type="email" id="email" class="w-full form-input" v-model="customer.email" required>
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

<script>
export default {
  data() {
    return {
      customer: {
        name: '',
        email: ''
      }
    }
  },
  methods: {
    addCustomer() {
      this.$emit('add-customer', this.customer);
      this.closeModal();
    },
    closeModal() {
      this.$emit('close');
    }
  }
}
</script>