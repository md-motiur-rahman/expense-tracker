<template>
  <h3>Add New Transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Name</label>
      <input
        type="text"
        id="text"
        v-model="text"
        placeholder="Transaction name"
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />(negative-expense, positive-income)</label
      >
      <input type="text" id="amount" v-model="amount" placeholder="Amount" />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";
const text = ref("");
const amount = ref("");

const emit = defineEmits(['transactionSubmitted'])

const toast = useToast();

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Both fields are required");
    return;
  }
  
  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit("transactionSubmitted", transactionData)

  text.value = "";
  amount.value = "";
};
</script>
