<script setup>
  const props=defineProps({
    currentBalance:{
      type:String,
      default:"0",
    }
  })
</script>

<template>
  <h4>Your Balance</h4>
  <h1 id="balance">${{ currentBalance }}</h1>
</template>