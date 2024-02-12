<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <InExp :income="+income" :expenses="+expenses" />
    <TransList :transactions="transactions" @transactionDeleted="deleteTrans" />
    <AddInEx @transactionSubmitted="handleTransaction" />
  </div>
</template>
<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import InExp from "./components/InEx.vue";
import TransList from "./components/TransList.vue";
import AddInEx from "./components/AddInEx.vue";

import { useToast } from "vue-toastification";
import { ref, computed, onMounted } from "vue";

const Toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransaction) {
    transactions.value = savedTransaction;
  }
});

//Get total balance
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

//Get Income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});

//Get Expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});

const handleTransaction = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  savedTransactionLocalStorage();
  Toast.success("Transaction added successfully");
};

const generateUniqueId = () => {
  return Math.floor(Math.random() * 10000);
};

const deleteTrans = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  savedTransactionLocalStorage();
  Toast.success("Transaction deleted successfully");
};

const savedTransactionLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
