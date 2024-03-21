<script setup>
import {ref, defineEmits} from "vue";
import {useToast} from "vue-toastification";

const toast = useToast()

const amount = ref(null)
const transactionText = ref('');
const emitFormSubmitted = defineEmits(["formSubmitted"])

const handleFormSubmit = ()=>{
  emitFormSubmitted("formSubmitted", {
    id: Date.now(),
    amount: amount.value,
    text: transactionText.value
  })

  if(amount.value > 0){
    toast.success('Income Added')
  } else {
    toast.warning('Expense Added')
  }
  amount.value = null;
  transactionText.value = '';





}


</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleFormSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="transactionText" type="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
      >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input v-model="amount" type="text" id="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>