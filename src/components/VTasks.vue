<template>
  <div class="flex justify-between items-center p-4">
    <div class="flex items-center gap-2">
      <input
        type="checkbox"
        name="fait"
        id="0"
        :todo="todo"
        @click="emitclickElementEvent"
        :checked="todo.state == 'finish'"
      />
      <span :class="{ 'line-through italic': todo.state == 'finish' }">{{
        todo.label
      }}</span>
    </div>
  </div>
</template>
<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps(["todo"]);
const emits = defineEmits(["clickElement"]);
function emitclickElementEvent() {
  emits("clickElement", props.todo);
}
</script>
