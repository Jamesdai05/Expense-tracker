<script setup>


  const props=defineProps({
    balance:{
      type:String,
      default:"0",
    }
  })
</script>

<template>
  <h4>Your Balance</h4>
  <h1 id="balance">${{ balance }}</h1>
</template>