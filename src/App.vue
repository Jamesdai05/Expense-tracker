<script setup>
import { computed, onMounted, ref } from 'vue';
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import History from "./components/History.vue";
import IncomeExpense from './components/IncomeExpense.vue';
import AddTransaction from './components/AddTransaction.vue';
import { useToast } from 'vue-toastification';
import { all } from 'axios';

// const transactions=[
//   {id:1,text:"Flower",amount:-29.99},
//   {id:2,text:"Salary",amount: 4500.00},
//   {id:3,text:"Book",amount:-19.99},
//   {id:4,text:"Play Station",amount:-699.99},
// ]

const toast=useToast();

let text=ref("");
let amount=ref("")
const allTransactions=ref([]);

onMounted(()=>{
  const savedTransactions=JSON.parse(localStorage.getItem("transactions"));

  if(savedTransactions){
    allTransactions.value=savedTransactions;
  }
})


const balance=ref("1000")


const income=computed(()=>{
  return allTransactions.value.filter(t=>t.amount>0).reduce((a,c)=>a+c.amount,0)
})

// computed the expense
const expense=computed(()=>{
  return allTransactions.value.filter(t=>t.amount<0).reduce((a,c)=>a+c.amount,0)
})


const currentBalance=computed(()=>{
  const totalBalance=allTransactions.value.reduce((acc,cur)=>acc+cur.amount,0).toFixed(2)
  return (parseFloat(balance.value)+parseFloat(totalBalance)).toFixed(2);
})
// console.log(updatedBalance);
// add the event to the form
const onSubmit=(e)=>{
  e.preventDefault();
  const trimmedText = text.value.trim();
  const amountValue = amount.value.trim();
  if (!trimmedText || trimmedText.length < 2) {
    // alert('Please enter a valid description (at least 2 characters)');
    toast.error('Please enter a valid description (at least 2 characters)');
    return;
  }

  if (!amountValue || isNaN(parseFloat(amountValue))) {
    // alert('Please enter a valid amount');
    toast.error('Please enter a valid amount.')
    return;
  }
  const newTransaction={
    id:Date.now(),
    text:text.value,
    amount:parseFloat(amount.value),
  }
  allTransactions.value=[...allTransactions.value,newTransaction],
  text.value="";
  amount.value=""

  saveTransactionsToLocalStorage()
  
  toast.success("New item added successfully!")
}

const onDelete=(id)=>{
  allTransactions.value=allTransctions.value.filter(item=>item.id!==id);
  toast.success("Item has been added successfully!")
  saveTransactionsToLocalStorage()
}


// save data to Localstorage
const saveTransactionsToLocalStorage=()=>{
  localStorage.setItem("transactions",JSON.stringify(allTransactions.value))
}

</script>


<template>
  <Header />
  <div class="container">
    <Balance :currentBalance="currentBalance"/>
    <IncomeExpense :income="income" :expense="expense"/>
    <History :transactions="allTransactions" @delete-transaction="onDelete"/>
    <AddTransaction @submit="onSubmit" v-model:amount="amount" v-model:text="text" />
  </div>
</template>


