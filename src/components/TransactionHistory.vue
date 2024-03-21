<script setup>
import {defineProps, defineEmits} from 'vue'

const {transactionData} = defineProps(["transactionData"]);
const emitDelete = defineEmits(["emitDeleteTransaction"]);

const handlEemitDelete =(id)=>{
  emitDelete('emitDeleteTransaction', id)

}

</script>

<template>
  <h3>History</h3>

  <ul id="list" class="list">
     <li v-for="transaction in transactionData" :key="transaction.id" @click="handlEemitDelete(transaction.id)" :class="parseFloat(transaction.amount)>0 ? 'plus':'minus'">
      {{transaction.text}} <span>{{transaction.amount}}$</span><button class="delete-btn">x</button>
    </li>
  </ul>
</template>