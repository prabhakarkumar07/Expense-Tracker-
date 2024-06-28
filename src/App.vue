<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <Income :income="+income" :expense="+expense" />
    <Transaction
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmited="handleTransactionSubmited" />
    <p class="cpywrite">
      Developed by ©
      <a href="https://www.linkedin.com/in/prabhakar-kumar-040644214/">
        Prabhakar kumar</a
      >
    </p>
  </div>
</template>
<style scoped>
.cpywrite {
  text-align: center;
  font-size: 10px;
}
</style>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import Income from "./components/Income.vue";
import Transaction from "./components/Transaction.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([]);
//saving to local storage
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});
// Get total
const total = computed(() => {
  return transactions.value.reduce(
    (acc, transaction) => acc + transaction.amount,
    0
  );
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

// Get expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

// Add transaction
const handleTransactionSubmited = (transactionData) => {
  transactions.value.push({
    id: genrateId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  saveTransactionsToLocalStorage();
  toast.success("Transaction Added");
};

// Generate unique id
const genrateId = () => {
  return Math.floor(Math.random() * 10000000000);
};

// Delete Transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionsToLocalStorage();
  toast.success("Transaction Deleted");
};

//save to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
