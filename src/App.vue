<template>
<Header />
<div class="container">
    <Balance :total="total"/>
    <IncomeExpense :income="income" :expense="expense"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"  />
    <AddTransaction  @transactionSubmitted="handleTransactionSubmitted" />
    
  
</div>
</template>
<script setup>

import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpense from './components/IncomeExpense.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'

import { ref, computed, onMounted  } from 'vue'

import { useToast } from "vue-toastification";


const transactions =ref([]);


onMounted(()=>{
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if(savedTransactions){
    transactions.value = savedTransactions;
  }
})






// Get total
const total = computed(() => {
    return transactions.value.reduce((acc, item) => (acc += item.amount), 0).toFixed(2);
  });

  // Get income

  const income = computed(() => {
    return transactions.value
      .filter((item) => item.amount > 0)
      .reduce((acc, item) => (acc += item.amount), 0)
      .toFixed(2);
  });

  // Get expense

  const expense = computed(() => {
    return (
      transactions.value
        .filter((item) => item.amount < 0)
        .reduce((acc, item) => (acc += item.amount), 0) * -1
    ).toFixed(2);
  });

  // Add transaction

  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push(
      {
        id: Math.floor(Math.random() * 100000000),
        text: transactionData.text,
        amount: transactionData.amount,
      }
    );
      saveToLocalStorage();

   useToast().success('Transaction Added');
  };

  // Delete transaction

  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
      (transaction) => transaction.id !== id
    );
    saveToLocalStorage();
    useToast().success('Transaction Deleted');
  };


  // Save to local storage

  const saveToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  };

</script>