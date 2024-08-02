<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { computed, ref } from "vue";

const transactions = ref([
  { id: 1, text: "Car", amount: 200 },
  { id: 2, text: "Car", amount: 200 },
  { id: 2, text: "Car", amount: -33 },
]);

const total = computed(() => {
  return transactions.value.reduce(
    (acc, transaction) => acc + transaction.amount,
    0
  );
});
</script>

<template>
  <Header></Header>
  <div class="container">
    <Balance :total="total"></Balance>
    <IncomeExpenses></IncomeExpenses>
    <TransactionList :transactions="transactions"></TransactionList>
    <AddTransaction></AddTransaction>
  </div>
</template>

<style scoped></style>
