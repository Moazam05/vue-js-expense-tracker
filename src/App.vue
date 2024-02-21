<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList
      :transactions="transactions"
      :deleteTransaction="deleteTransaction"
    />
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
import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

// states
const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const addTransaction = (transaction) => {
  const data = {
    id: transactions?.value.length + 1,
    text: transaction.text,
    amount: +transaction.amount,
  };

  transactions.value.push(data);

  localStorage.setItem("transactions", JSON.stringify(transactions.value));
  toast.success("Transaction added successfully");
};

const deleteTransaction = (id) => {
  const deleteTransaction = transactions.value.filter((item) => item.id !== id);

  transactions.value = deleteTransaction;

  localStorage.setItem("transactions", JSON.stringify(transactions.value));
  toast.success("Transaction deleted successfully");
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
