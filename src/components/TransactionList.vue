<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
      {{ transaction.text }} <span> Rs{{ transaction.amount }}</span>
      <button @click="deleteTransaction(transaction.id)" 
      class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
  import { defineProps } from 'vue';

  const props = defineProps({
    transactions: {
      type: Array,
      required: true,
    }
  });

  const emits = defineEmits(['deleteTransaction'])

  // Delete Transaction
  const deleteTransaction = (id) => {
    emits('deleteTransaction', id)
  }

</script>

<!-- With OPTION'S API -->
<!-- <script>
export default {
  data() {
    return {
      transactions: [
        { id: 1, text: 'Books', amount: -20 },
        { id: 2, text: 'Salary', amount: 200 },
        { id: 3, text: 'Rent', amount: -70 },
        { id: 4, text: 'Freelance', amount: 300 },
        { id: 5, text: 'Flowers', amount: -5.77 },
        { id: 6, text: 'Grocery', amount: -34 },
      ]
    }
  }
}
</script> -->
