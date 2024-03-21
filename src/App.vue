<script setup>

import TransactionHistory from "./components/TransactionHistory.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import AddTransaction from "./components/AddTransaction.vue";
import {ref, computed, onMounted} from "vue";

const transactionData = ref([]);

onMounted(()=>{
  const transactions =  localStorage.getItem('transactions');
  if(transactions){
    transactionData.value = JSON.parse(transactions)
  }
})
const handleFormData = (data)=>{
  console.log(data)
  transactionData.value.push(data);
  localStorage.setItem('transactions', JSON.stringify(transactionData.value))

}

const total = computed(()=>{
  return transactionData.value.reduce((acc, curr)=> acc+ parseFloat(curr.amount),0)
})

const income = computed(()=>{
  // console.log('inside computed', totalIncome.value)
   return transactionData.value.filter(transaction=> parseFloat(transaction.amount) > 0).reduce((acc, current)=> acc + parseFloat(current.amount),0);

})

const expense = computed(()=>{
  return transactionData.value.filter(transaction=> parseFloat(transaction.amount) < 0).reduce((acc, current)=> acc + parseFloat(current.amount),0);


})

const handleEmitDelete =(id)=>{
  console.log(id)
  transactionData.value = transactionData.value.filter(transaction => transaction.id !== id);
  localStorage.setItem('transactions', JSON.stringify(transactionData.value))
}

</script>

<template>
  <h2>Expense Tracker</h2>
  <div class="container">
    <h1 id="balance">${{total}}</h1>
    <IncomeExpense :income="income" :expense="expense"></IncomeExpense>
    <TransactionHistory @emitDeleteTransaction="handleEmitDelete" :transactionData="transactionData"></TransactionHistory>
    <AddTransaction @formSubmitted="handleFormData"></AddTransaction>
  </div>
</template>

<style scoped>

</style>
