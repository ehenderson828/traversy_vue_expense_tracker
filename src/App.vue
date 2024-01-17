<template>
    <Header />
    <div class="container">
        <!-- ':total' is being passed into the balance component as a prop. See the 'total' variable below-->
        <Balance :total="+total"/>
        <!-- To pass variables in as numbers, you need to add a '+' sign -->
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <!-- ':transactions' is also being passed to the component as a prop. See the 'transactions' array below-->
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>

<!-- With the use of our 'setup' keyword, there's no need to register any of our components -->
<script setup>
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import TransactionList from './components/TransactionList.vue';
    import AddTransaction from './components/AddTransaction.vue';

    import { useToast } from 'vue-toastification';

    const toast = useToast();

    // 'ref' function for reactivity
    import { ref } from 'vue';
    // 'computed' property to create reactive and cached values
    import { computed } from 'vue';
    // Lifecycle method after component mounts
    import { onMounted } from 'vue';

    // Transactions List
    // The ref functions makes this array reactive if we change a value
    const transactions = ref([]);

    onMounted(() => {
        const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

        if(savedTransactions) {
            transactions.value = savedTransactions;
        }
    });

    // Get total
    const total = computed(() => { // 'computed()' always takes in a function as a parameter
        return transactions.value.reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0); // We're passing in '0' here as a place for the accumulator to start. The accumulator will then add all of the transaction amounts to that starting point
    });
    // reduce() is useful here because we're just adding all of the transaction amounts together, positive or negative

    // Get Income
    const income = computed(() => { // 'computed()' always takes in a function as a parameter
        return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0).toFixed(2); // 'toFixed()' provides us with decimal places according to the value passed
    });

    // Get Expenses
    const expenses = computed(() => {
        return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0).toFixed(2);
    });

    // Add Transaction
    const handleTransactionSubmitted = (transactionData) => {
        transactions.value.push({
            id: generateUniqueId(),
            text: transactionData.text,
            amount: transactionData.amount
        });

        saveTransactionsToLocalStorage();

        toast.success('Transaction added');

    };

    // Generate unique id
    const generateUniqueId = () => {
        return Math.floor(Math.random() * 1000000);
    };

    // Delete Transaction
    const handleTransactionDeleted = (id) => {
        transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

        saveTransactionsToLocalStorage();

        toast.success('Transaction deleted');
    };

    // Save to localstorage
    const saveTransactionsToLocalStorage = () => {
        localStorage.setItem('transactions', JSON.stringify(transactions.value));
    }

    // export default {
    //     components: {
    //         Header,
    //         Balance,
    //         IncomeExpenses,
    //         TransactionList,
    //         AddTransaction,
    //     },
    // };
</script>

<!-- All of the style for this project is located in  'style.css'. None of it is scoped inside one componet and is completely global-->