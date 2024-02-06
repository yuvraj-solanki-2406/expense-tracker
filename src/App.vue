<template>
  <Header></Header>
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <TransactionList :transactions="transactions" @deleteTransaction="deleteTransaction"/>
    <AddTransaction @transactionSubmitted="handleSubmit" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { useToast } from 'vue-toastification';

const toast = useToast()

// Logic Script
import { ref, computed, onMounted } from 'vue';

const transactions = ref([]);

// Connects localstorage with website
onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem
  ('transactions'));

  if(savedTransaction) {
    transactions.value = savedTransaction;
  }
});

// Get Total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get Income
const income = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Get Expenses
const expense = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Add new transactions
const handleSubmit = (transaction_data) => {
  // console.log(transaction_data)
  transactions.value.push({
    id:generateUniqueId(),
    text: transaction_data.text,
    amount: transaction_data.amount,
  });
  saveTransactiontoLocalStorage();
  toast.success("Transaction Added");
};

// Generate Unique Id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

// Delete Transaction
const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id 
  );
  saveTransactiontoLocalStorage();
  toast.success("Transaction Deleted")
}

// Add in localstorage
const saveTransactiontoLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>