<template>
  <div class="bg-white items-center flex justify-center h-2/3 mt-20 flex-col">
    <div class="shadow-md flex-col flex rounded overflow-hidden w-80 h-96">
      <div class="relative bg-violet-200">
        <div class="flex text-white px-6 py-10 gap-2">
          <span class="font-bold text-6xl">{{ numberTasksDo }}</span>
          <div>
            <div class="font-semibold">Tasks</div>
            <div>/{{ numberTasks }}</div>
          </div>
        </div>
        <button
          class="-bottom-6 right-5 rounded-full absolute p-6 bg-blue-500 text-white mb-4 leading-4"
          @click="click"
          v-if="!isClick"
        ></button>
      </div>
      <div class="mt-4">
        <VAdd :isClick="isClick" @addElement="addToDo" />
      </div>
      <div class="flex justify-between px-4">
        <span
          class="px-4"
          @click="etat = 'tout'"
          :class="{ 'border-b border-blue-600': etat == 'tout' }"
          >Tout</span
        >
        <span
          @click="etat = 'fait'"
          :class="{ 'border-b border-blue-600': etat == 'fait' }"
          >Fait</span
        >
        <span
          class="px-4"
          @click="etat = 'nofait'"
          :class="{ 'border-b border-blue-600': etat == 'nofait' }"
          >No Fait</span
        >
      </div>
      <div class="overflow-scroll">
        <div class="flex flex-col justify-center" v-if="etat == 'tout'">
          <VTasks
            :todo="todo"
            v-for="(todo, index) in allToDo"
            :key="index"
            @clickElement="toggleTodo"
          />
        </div>
        <div class="flex flex-col justify-centere" v-if="etat == 'fait'">
          <VTasks
            :todo="todo"
            v-for="(todo, index) in finishToDo"
            :key="index"
            @clickElement="toggleTodo"
          />
        </div>
        <div class="flex flex-col justify-center" v-if="etat == 'nofait'">
          <VTasks
            :todo="todo"
            v-for="(todo, index) in notToDo"
            :key="index"
            @clickElement="toggleTodo"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, computed } from "vue";
import VAdd from "@/components/VAdd.vue";
import VTasks from "@/components/VTasks.vue";

let isClick = ref(false);
const allToDo = ref([]);
const etat = ref("tout");

const notToDo = computed(() => {
  return allToDo.value.filter((el) => {
    return el.state == "progress";
  });
});
const finishToDo = computed(() => {
  return allToDo.value.filter((el) => {
    return el.state == "finish";
  });
});
const numberTasks = computed(() => {
  return allToDo.value.length;
});
const numberTasksDo = computed(() => {
  return finishToDo.value.length;
});
function click() {
  isClick.value = true;
}

function addToDo(todo) {
  allToDo.value.push(todo);
  console.log("allToDo", allToDo);
  isClick.value = false;
}

function toggleTodo(todo) {
  if (todo.state == "progress") {
    todo.state = "finish";
  } else {
    todo.state = "progress";
  }
}
</script>

<style lang="scss" scoped></style>
