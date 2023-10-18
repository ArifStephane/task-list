<template>
  <div class="flex p-4 items-stretch border-b" v-if="isClick">
    <form class="flex gap-4" @submit.prevent="emitAddElementEvent">
      <input
        v-model="todo.label"
        type="text"
        name="in"
        class="border-blue-400 border rounded"
      />
      <button class="px-2 py-1 font-bold rounded text-white bg-blue-500" type="submit">
        Add task
      </button>
    </form>
  </div>
</template>
<script setup>
import { ref, defineProps } from "vue";
defineProps(["isClick"]);
const todo = ref({ state: "progress", label: ""});
const emits = defineEmits(["addElement"]);

function emitAddElementEvent() {
  emits("addElement", todo.value);
  todo.value = { state: "progress", label: "" };
}
</script>
