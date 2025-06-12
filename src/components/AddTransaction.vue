<script setup>
const props=defineProps({
  amount:{
    type:String,
    default:"0",
    required:true,
  },
  text:{
    type:String,
    required:true,
  }
});
defineEmits(['update:text','update:amount','submit'])
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit="$emit('submit',$event)">
    <div class="form-control">
      <label for="text">Item</label>
      <input type="text" id="text" placeholder="Enter text..." :value="text" @input="$emit('update:text',$event.target.value)"/>
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label>
      <input type="text" id="amount" placeholder="Enter amount..." :value="amount" @input="$emit('update:amount',$event.target.value)" />
    </div>
    <button class="btn" type="submit">Add transaction</button>
  </form>
</template>