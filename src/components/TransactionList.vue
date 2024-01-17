<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <!-- ':v-for' is our directive to loop through each object in out array 'transactions' -->
        <!-- Our ':key' or 'v-bind:key' directive is a unique value that can be iterated over. It's required for the loop to work -->
        <!-- The ':class' directive works with a ternary to add classes bases on the expense amount -->
        <li 
            v-for="transaction in transactions" 
            :key="transaction.id" 
            :class="transaction.amount < 0 ? 'minus' : 'plus'"
        >
            {{ transaction.text }} <span>${{ transaction.amount }}</span><button @click="deleteTransaction(transaction.id)" class="delete-btn"></button>
        </li>
    </ul>
</template>

<script setup>
    import { defineProps } from 'vue';

    const emit = defineEmits(['transactionDeleted']);

    const props = defineProps({
        transactions: {
            type: Array,
            required: true,
        }
    });

    const deleteTransaction = (id) => {
        emit('transactionDeleted', id);
    };
</script>

<!-- The 'setup' keyword is the newest syntax for data and methods using the composition api: all you need is the keyword and the data. We don't have to return and we don't have to use a setup() or an export default-->
<!-- <script setup>
    //Examples of options api and composition api
    // export default {
        // This is an example of how the 'options api' works with parent level components
        // Data is defined here, but should be able to be access through our 'template'
        // data () {
        //     return {
        //         // Since this is an array, we should be able to loop through this and output a list item for each object with the use of a 'directive' (binding for loops, conditionals)
        //         transactions: [
        //             { id: 1, text: 'Flower', amount: -19.99 },
        //             { id: 2, text: 'Salary', amount: 299.97 },
        //             { id: 3, text: 'Book', amount: -10 },
        //             { id: 4, text: 'Camera', amount: 150 },
        //         ],
        //     };
        // },

        // This, on the other hand, is how the composition api works
    //     setup() {
    //         const transactions = [
    //             { id: 1, text: 'Flower', amount: -19.99 },
    //             { id: 2, text: 'Salary', amount: 299.97 },
    //             { id: 3, text: 'Book', amount: -10 },
    //             { id: 4, text: 'Camera', amount: 150 },
    //         ];
            
    //         return {
    //             transactions,
    //         };
    //     },
    // };
    
    // With the 'setup' keyword in the script tag, all we need is the decalred data
    const transactions = [
        { id: 1, text: 'Flower', amount: -19.99 },
        { id: 2, text: 'Salary', amount: 299.97 },
        { id: 3, text: 'Book', amount: -10 },
        { id: 4, text: 'Camera', amount: 150 },
    ];
</script> -->