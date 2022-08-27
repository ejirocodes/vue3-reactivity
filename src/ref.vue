<script setup lang="ts">
import { ref } from "vue";

let count = ref(3);
let countCopy = $ref(count);

function incrementCount() {
  count.value++;
}
function incrementCopy() {
  countCopy++;
}
</script>
<template>
  <div>
    <h1>ref()</h1>
    <div>Copy count {{ countCopy }}</div>
    <button @click="incrementCopy">Increment copy</button>
    <br />
    <br />
    <div>Count {{ count }}</div>
    <button @click="incrementCount">Increment count</button>
  </div>
</template>

<style scoped></style>
