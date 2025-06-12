<script setup lang="ts">


  interface Transaction {
  id: number
  text: string
  amount: number
}
const props = defineProps<{
  transactions: Transaction[],
}>()
const emit = defineEmits<{
  (e: 'delete-transaction', id: number): void;
}>();

// const handleDelete = (id:number) => {
//   emit('delete-transaction', id);
// };
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li :class="{'minus':transaction.amount<0,'plus':transaction.amount>0}" v-for="transaction in transactions" :key="transaction.id">
      <!-- {{ transaction.text }} <span>{{ transaction.amount }}</span><button class="delete-btn" type="submit" @click="handleDelete(transaction.id)">x</button> -->
      {{ transaction.text }} <span>{{ transaction.amount }}</span><button class="delete-btn" type="submit" @click="$emit('delete-transaction',transaction.id)">x</button>
      <!-- Cash <span>-$400</span><button class="delete-btn">x</button> -->
    </li>
  </ul>
</template>