<template>
  <h4>Your Balance</h4>
  <h1 id="balance" :class="balanceClass">₹{{ total }}</h1>
</template>

<script setup>
import { defineProps, computed } from "vue";

const props = defineProps({
  total: {
    type: Number,
    required: true,
  },
});

const balanceClass = computed(() => {
  if (props.total < 0) {
    return "red";
  } else if (props.total > 0) {
    return "green";
  } else {
    return "zero";
  }
});
</script>

<style scoped>
.red {
  color: red;
}
.green {
  color: green;
}
.zero {
  color: black;
}
</style>
