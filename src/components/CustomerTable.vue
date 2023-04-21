<template>
<div class="p-4">
    <div class="flex flex-wrap items-center justify-between mb-4">
        <div class="flex items-center mb-2 filters md:mb-0">
            <label class="mr-2">Label:</label>
            <select v-model="labelFilter" class="p-2 bg-gray-100 rounded-lg">
                <option value="">All</option>
                <option v-for="label in labels" :value="label">{{ label }}</option>
            </select>
            <label class="ml-4 mr-2">Branch:</label>
            <select v-model="branchFilter" class="p-2 bg-gray-100 rounded-lg">
                <option value="">All</option>
                <option v-for="branch in branches" :value="branch">{{ branch }}</option>
            </select>
            <label class="ml-4 mr-2">Sales Rep:</label>
            <select v-model="salesRepFilter" class="p-2 bg-gray-100 rounded-lg">
                <option value="">All</option>
                <option v-for="rep in salesReps" :value="rep">{{ rep }}</option>
            </select>
        </div>

        <div class="flex items-center search">
            <label class="mr-2">Search:</label>
            <input v-model="searchTerm" type="text" class="p-2 bg-gray-100 rounded-lg">
        </div>
    </div>

    <table class="w-full border border-collapse border-gray-300">
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
            </tr>
        </tbody>
    </table>
</div>
</template>

<script>
import customers from "../customers.json";
export default {
    data() {
        return {
            customers,
            labelFilter: "",
            branchFilter: "",
            salesRepFilter: "",
            searchTerm: "",
        };
    },

    computed: {
        filteredCustomers() {
            return this.customers.filter((customer) => {
                const labelFilter =
                    this.labelFilter === "" || customer.label === this.labelFilter;
                const branchFilter =
                    this.branchFilter === "" || customer.branch === this.branchFilter;
                const salesRepFilter =
                    this.salesRepFilter === "" || customer.salesRep === this.salesRepFilter;
                const searchTerm =
                    this.searchTerm === "" ||
                    customer.name.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
                    customer.phone.includes(this.searchTerm) ||
                    customer.email.toLowerCase().includes(this.searchTerm.toLowerCase());
                return labelFilter && branchFilter && salesRepFilter && searchTerm;
            });
        },

        labels() {
            const allLabels = this.customers.map((customer) => customer.label);
            return [...new Set(allLabels)];
        },

        branches() {
            const allBranches = this.customers.map((customer) => customer.branch);
            return [...new Set(allBranches)];
        },

        salesReps() {
            const allReps = this.customers.map((customer) => customer.salesRep);
            return [...new Set(allReps)];
        },
    },
};
</script>
