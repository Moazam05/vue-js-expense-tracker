<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
// imports
import { useToast } from "vue-toastification";
import { defineProps } from "vue";

// props
const props = defineProps({
  addTransaction: Function,
});
const { addTransaction } = props;

// states
const toast = useToast();

const onSubmit = () => {
  if (text.value === "" || amount.value === "") {
    toast.error("Both fields must be filled");
    return;
  }

  const payload = {
    text: text.value,
    amount: amount.value,
  };

  addTransaction(payload);

  text.value = "";
  amount.value = "";
};
</script>

<script>
export default {
  name: "AddTransaction",
};
</script>
