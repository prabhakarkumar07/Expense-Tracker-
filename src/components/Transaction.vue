<template>
  <h3>History</h3>
  <div id="list" class="list">
    <li
      v-for="transaction in visibleTransactions"
      v-bind:key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>{{ transaction.amount }} </span
      ><button class="delete-btn" @click="deleteTransaction(transaction.id)">
        X
      </button>
    </li>
  </div>
  <div class="btn-container">
    <button v-if="showViewMore" @click="toggleView" class="view-more-btn">
      View More
    </button>
    <button v-if="showViewLess" @click="toggleView" class="view-less-btn">
      View Less
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["transactionDeleted"]);
const deleteTransaction = (id) => {
  emit("transactionDeleted", id);
};

const showAll = ref(false);

const visibleTransactions = computed(() => {
  return showAll.value ? props.transactions : props.transactions.slice(0, 4);
});

const showViewMore = computed(() => {
  return props.transactions.length > 4 && !showAll.value;
});

const showViewLess = computed(() => {
  return props.transactions.length > 4 && showAll.value;
});

const toggleView = () => {
  showAll.value = !showAll.value;
};
</script>

<style scoped>
.minus {
  color: red;
}

.plus {
  color: green;
}
.btn-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.view-more-btn,
.view-less-btn {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 5px;
  margin: 0 5px;
}

.view-more-btn:hover,
.view-less-btn:hover {
  background-color: #0056b3;
}
</style>
