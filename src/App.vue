<template>
   <div>
    <CustomerTable />
  </div>
</template>

<script setup>
  import CustomerTable from './components/CustomerTable.vue'
</script>

<script>
  // Export your component directly
  export default {
    components: {
      CustomerTable
    }
  }
</script>

<style>
  /* Your styles here */
</style>