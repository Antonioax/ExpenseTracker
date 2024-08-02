<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { computed, onMounted, ref } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([]);

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

const onTransaction = (transaction) => {
  transactions.value.unshift({
    id: generateUniqueId(),
    text: transaction.text,
    amount: transaction.amount,
  });

  updateStorage();
  toast.success("Transaction added");
};

const allIds = new Set(transactions.value.map((t) => t.id));

const generateUniqueId = () => {
  let id;
  do {
    id = Math.floor(Math.random() * 1000000);
  } while (allIds.has(id));

  allIds.add(id);
  console.log(allIds);
  return id;
};

const onDelete = (id) => {
  transactions.value = transactions.value.filter((t) => t.id !== id);
  allIds.delete(id);
  updateStorage();
  toast.success("Transaction deleted");
};

const updateStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransaction) transactions.value = savedTransaction;
});
</script>

<template>
  <Header></Header>
  <div class="container">
    <Balance :total="total"></Balance>
    <IncomeExpenses :income="+income" :expense="+expense"></IncomeExpenses>
    <AddTransaction @transaction="onTransaction"></AddTransaction>
    <TransactionList
      :transactions="transactions"
      @deleted="onDelete"
    ></TransactionList>
  </div>
</template>

<style scoped></style>
