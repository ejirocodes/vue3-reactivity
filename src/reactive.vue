<script setup lang="ts">
import { reactive } from "vue";

const state = reactive({ count: 3 });
let copy = reactive({ count: state.count });

// Destructuring causes reactivity loss
let { count } = state;

function incrementCount() {
  state.count++;
  count++;
}

function incrementCopy() {
  copy.count++;
}

// const formData = reactive({
//   name: "",
//   email: "",
//   password: "",
// });
</script>
<template>
  <div>
    <h1>reactive()</h1>
    <div>Copy count {{ copy.count }}</div>
    <button @click="incrementCopy">Increment copy</button>
    <br />
    <br />
    <div>Count {{ state.count }}</div>
    <div>Count destructuring {{ count }}</div>
    <button @click="incrementCount">Increment count</button>
  </div>
</template>

<style scoped></style>
