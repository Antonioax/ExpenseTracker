<script setup>
defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["deleted"]);

const onDelete = (id) => {
  emit("deleted", id);
};
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn" @click="onDelete(transaction.id)">X</button>
    </li>
  </ul>
</template>
