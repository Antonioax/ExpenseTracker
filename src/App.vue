<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { computed, ref } from "vue";

const transactions = ref([
  { id: 1, text: "Car", amount: 200.22 },
  { id: 2, text: "Car", amount: 200 },
  { id: 2, text: "Car", amount: -33 },
]);

const total = computed(() => {
  return transactions.value.reduce(
    (acc, transaction) => acc + transaction.amount,
    0
  );
});

const income = computed(() => {
  return transactions.value
    .filter((t) => t.amount > 0)
    .reduce((acc, t) => acc + t.amount, 0)
    .toFixed(2);
});

const expense = computed(() => {
  return transactions.value
    .filter((t) => t.amount < 0)
    .reduce((acc, t) => acc + t.amount, 0)
    .toFixed(2);
});


</script>

<template>
  <Header></Header>
  <div class="container">
    <Balance :total="total"></Balance>
    <IncomeExpenses :income="+income" :expense="+expense"></IncomeExpenses>
    <TransactionList :transactions="transactions"></TransactionList>
    <AddTransaction></AddTransaction>
  </div>
</template>

<style scoped></style>
