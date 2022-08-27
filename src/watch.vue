<script setup lang="ts">
import { reactive, ref, watch, watchEffect } from "vue";

let count = ref(3);
let todoId = ref(5);
let countCopy = ref(count);
const state = reactive({ count: 3 });

function incrementCount() {
  count.value++;
}
function incrementCopy() {
  countCopy.value++;
}

// Watching a ref
watch(count, (count, prevCount) => {
  console.log({ count, prevCount });
});

// Watching a ref  #alternative syntax (getter)
watch(
  () => count.value + countCopy.value,
  (count) => {
    console.log({ count });
  },
  {}
);

watch(
  [count, countCopy],
  (
    [val1, val2] // [oldVal1, oldVal2]
  ) => {
    console.log({ val1, val2 });
  }
);

// Watching a reactive object  (getter)
// watch(
//   () => state.count,
//   (count, prevCount) => {
//     console.log({ count, prevCount });
//   }
// );
// watch third optional argument
watch(
  () => state,
  (count) => {
    console.log("third optional argument --> watch", count);
  },
  {
    immediate: true,
    deep: true,
  }
);

// watchEffect

interface Todo {
  completed: boolean;
  id: number;
  title: string;
  userId: number;
}
const todo = ref<Todo | null>(null);
// fetch immediately
// ...then watch for id (todoId) change
watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  );
  const data: Todo = await response.json();
  todo.value = data;
  console.log(todo.value);
});
</script>
<template>
  <div>
    <h1>watch()</h1>
    <div>Copy count {{ countCopy }}</div>
    <button @click="incrementCopy">Increment copy</button>
    <br />
    <br />
    <div>Count {{ count }}</div>
    <button @click="incrementCount">Increment count</button>
    <br />
    <br />
    <div>Count (reactive) {{ state.count }}</div>
    <button @click="state.count++">Increment count</button>
    <br />
    <br />
    <div>
      <p>{{ todo?.id }}</p>
      <p>{{ todo?.title }}</p>
      <p>{{ todo?.completed }}</p>
    </div>
    <div>WatchEffect {{ todoId }}</div>
    <button @click="todoId++">Increment todoId</button>
  </div>
</template>

<style scoped></style>
