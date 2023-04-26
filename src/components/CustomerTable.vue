<script lang="ts">
import {
    defineComponent,
    ref,
    computed,
    reactive
} from "vue";
import Pagination from "v-pagination-3";
import axios from "axios";

import AddCustomerModal from "./AddCustomerModal.vue";
import EditCustomerModal from "./EditCustomerModal.vue";

interface Customer {
    id: number;
    name: string;
    phone: string;
    email: string;
    label: string;
    branch: string;
    salesRep: string;
}

export default defineComponent({
    components: {
        AddCustomerModal,
        EditCustomerModal,
        Pagination,
    },

    setup() {
        const customers = reactive < Customer[] > ([]);
        const labelFilter = ref("");
        const branchFilter = ref("");
        const salesRepFilter = ref("");
        const searchTerm = ref("");
        const currentPage = ref(1);
        const perPage = ref(5);
        const showAddCustomerModal = ref(false);
        const editCustomerModalVisible = ref(false);
        const selectedCustomer = ref < Customer | null > (null);

        const addCustomer = (customer: Customer) => {
            // Add the new customer to the list
            customers.push(customer);
        };

        const handleCustomerAdded = (customer: Customer) => {
            customers.push(customer);
        };

        const editCustomer = (customer: Customer) => {
            selectedCustomer.value = customer;
            editCustomerModalVisible.value = true;
        };

        const updateCustomer = (updatedCustomer: Customer) => {
            selectedCustomer.value = updatedCustomer;

            const index = customers.findIndex(
                (customer) => customer.id === updatedCustomer.id
            );

            customers.splice(index, 1, updatedCustomer);
        };

        const filteredCustomers = computed(() => {
            return customers.filter((customer) => {
                const labelFilterValue =
                    labelFilter.value === "" || customer.label === labelFilter.value;
                const branchFilterValue =
                    branchFilter.value === "" || customer.branch === branchFilter.value;
                const salesRepFilterValue =
                    salesRepFilter.value === "" ||
                    customer.salesRep === salesRepFilter.value;
                const searchTermValue =
                    searchTerm.value === "" ||
                    customer.name.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
                    customer.phone.includes(searchTerm.value) ||
                    customer.email.toLowerCase().includes(searchTerm.value.toLowerCase());
                return (
                    labelFilterValue &&
                    branchFilterValue &&
                    salesRepFilterValue &&
                    searchTermValue
                );
            });
        });

        const paginatedCustomers = computed(() => {
            const start = (currentPage.value - 1) * perPage.value;
            const end = start + perPage.value;
            return filteredCustomers.value.slice(start, end);
        });

        const labels = computed(() => {
            const allLabels = customers.map((customer) => customer.label);
            return [...new Set(allLabels)];
        });

        const branches = computed(() => {
            const allBranches = customers.map((customer) => customer.branch);
            return [...new Set(allBranches)];
        });

        const salesReps = computed(() => {
            const allReps = customers.map((customer) => customer.salesRep);
            return [...new Set(allReps)];
        });

        axios
            .get("../db.json")
            .then((response) => {
                customers.push(...response.data.customers);
            })
            .catch((error) => {
                console.log(error);
            });

        return {
            customers,
            labelFilter,
            branchFilter,
            salesRepFilter,
            searchTerm,
            currentPage,
            perPage,
            showAddCustomerModal,
            editCustomerModalVisible,
            selectedCustomer,
            addCustomer,
            editCustomer,
            handleCustomerAdded,
            updateCustomer,
            filteredCustomers,
            paginatedCustomers,
            labels,
            branches,
            salesReps,
        };

    },
});
</script>

<template>
<div class="p-4">
    <h1 class="mb-6">Customer List</h1>
    <div class="flex flex-wrap items-center justify-between mb-4">
        <div class="flex items-center mb-2 filters md:mb-0">
            <label class="mr-2">Label:</label>
            <select v-model="labelFilter" class="p-2 bg-gray-100 rounded-lg">
                <option value="">All</option>
                <option v-for="label in labels" :value="label" :key="label">{{ label }}</option>
            </select>
            <label class="ml-4 mr-2">Branch:</label>
            <select v-model="branchFilter" class="p-2 bg-gray-100 rounded-lg">
                <option value="">All</option>
                <option v-for="branch in branches" :value="branch" :key="branch">{{ branch }}</option>
            </select>
            <label class="ml-4 mr-2">Sales Rep:</label>
            <select v-model="salesRepFilter" class="p-2 bg-gray-100 rounded-lg">
                <option value="">All</option>
                <option v-for="rep in salesReps" :value="rep" :key="rep">{{ rep }}</option>
            </select>
        </div>

        <div class="flex items-center search">
            <label class="mr-2">Search:</label>
            <input v-model="searchTerm" type="text" placeholder="Search by name, phone, or email" class="p-2 bg-gray-100 rounded-lg form-control">
        </div>
    </div>
    <div>
        <table class="w-full mb-4 border border-collapse border-gray-300" id="customerTable">
            <thead>
                <tr class="bg-gray-100 border-b border-gray-300">
                    <th class="p-2 text-left">ID</th>
                    <th class="p-2 text-left">Name</th>
                    <th class="p-2 text-left">Phone</th>
                    <th class="p-2 text-left">Email</th>
                    <th class="p-2 text-left">Label</th>
                    <th class="p-2 text-left">Branch</th>
                    <th class="p-2 text-left">Sales Rep</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="customer in filteredCustomers" :key="customer.id" class="border-b border-gray-300">
                    <td class="p-2">{{ customer.id }}</td>
                    <td class="p-2">{{ customer.name }}</td>
                    <td class="p-2">{{ customer.phone }}</td>
                    <td class="p-2">{{ customer.email }}</td>
                    <td class="p-2">{{ customer.label }}</td>
                    <td class="p-2">{{ customer.branch }}</td>
                    <td class="p-2">{{ customer.salesRep }}</td>
                    <td class="p-2">
                        <button @click="editCustomer(customer)" class="px-2 py-1 text-white bg-blue-500 rounded hover:bg-blue-700">Edit</button>
                        <EditCustomerModal v-if="editCustomerModalVisible" :customer="selectedCustomer" @close="editCustomerModalVisible = false" @update-customer="updateCustomer" />
                    </td>
                </tr>
            </tbody>
        </table>
        <div class="flex justify-center">
            <v-pagination-3 v-if="filteredCustomers.length > perPage" v-model="currentPage" :perPage="perPage" :total="filteredCustomers.length" />
        </div>
    </div>
    <div>
        <button @click="showAddCustomerModal = true" class="px-2 py-1 text-white bg-green-500 rounded hover:bg-green-700">Add Customer</button>
        <AddCustomerModal v-if="showAddCustomerModal" @close="showAddCustomerModal = false" @add-customer="addCustomer" :customerAdded="handleCustomerAdded" />
    </div>

</div>
</template>
