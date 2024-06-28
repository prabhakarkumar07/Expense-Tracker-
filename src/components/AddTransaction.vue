<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Name of Transaction</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text.." />
    </div>
    <div class="form-control">
      <label>Amount</label><br />
      <input
        type="radio"
        id="income"
        value="income"
        v-model="transactionType"
      />
      <label for="income">Income</label>

      <input
        type="radio"
        id="expense"
        value="expense"
        v-model="transactionType"
      />
      <label for="expense">Expense</label>

      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
        :class="{
          'income-amount': transactionType === 'income',
          'expense-amount': transactionType === 'expense',
        }"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");
const transactionType = ref("expense"); // Default to expense

const emit = defineEmits(["transactionSubmited"]);

const toast = useToast();
const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Both fields must be filled");
    return;
  }

  if (text.value.length > 75) {
    toast.error("Name of Transaction must not exceed 75 characters");
    return;
  }

  const parsedAmount = parseFloat(amount.value);
  if (isNaN(parsedAmount)) {
    toast.error("Amount must be a number");
    return;
  }

  // Adjust amount based on transaction type
  const adjustedAmount =
    transactionType.value === "expense" ? -parsedAmount : parsedAmount;

  const transactionData = {
    text: text.value,
    amount: adjustedAmount,
  };
  emit("transactionSubmited", transactionData);

  text.value = "";
  amount.value = "";
};
</script>

<style scoped>
.income-amount {
  border-color: green;
}
.expense-amount {
  border-color: red;
}
</style>
