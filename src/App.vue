<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList :transactions="transactions" />
    <AddTransaction :addTransaction="addTransaction" />
  </div>
</template>

<script setup>
// Custom imports
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
// imports
import { ref, computed } from "vue";
import { useToast } from "vue-toastification";

// states
const toast = useToast();

const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 199.97 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 150 },
]);

const addTransaction = (transaction) => {
  const data = {
    id: transactions?.value.length + 1,
    text: transaction.text,
    amount: +transaction.amount,
  };

  transactions.value.push(data);
  toast.success("Transaction added successfully");
};

// Get total
const total = computed(() => {
  return transactions.value
    .reduce((acc, item) => {
      return acc + item.amount;
    }, 0)
    .toFixed(2);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((item) => item.amount > 0)
    .reduce((acc, item) => {
      return acc + item.amount;
    }, 0)
    .toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return (
    transactions.value
      .filter((item) => item.amount < 0)
      .reduce((acc, item) => {
        return acc + item.amount;
      }, 0) * -1
  ).toFixed(2);
});
</script>
