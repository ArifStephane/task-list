<template>
  <div class="bg-white items-center flex justify-center max-h-screen  h-screen flex-col">
    <div class="font-bold text-[60px]">TodoApp</div>
    <div
      class="shadow-md flex-col flex rounded-[8px] border-4 border-white overflow-scroll overflow-x-hidden overflow-y-hidden xl:w-[500px] h-[500px] lg: w-[100%]"
    >
      <div class="relative bg-violet-200">
        <div class="flex text-white px-6 py-10 gap-2">
          <span class="font-bold text-6xl">{{ numberTasksDo }}</span>
          <div>
            <div class="font-bold">Tasks</div>
            <div class="font-bold">/{{ numberTasks }}</div>
          </div>
        </div>
        <div
          class="-bottom-6 right-5 rounded-full absolute p-2 bg-white text-white"
          @click="click"
          v-if="!isClick"
        >
          <img src="@/components/icons/icons8-add.gif" />
        </div>
      </div>
      <div class="mt-4">
        <VAdd :isClick="isClick" @addElement="addToDo" />
      </div>
      <div class="flex justify-between mt-4 px-4">
        <span
          class="px-4 text-lg font-bold text-center w-[80px]"
          @click="etat = 'tout'"
          :class="{ 'border-b-4 border-blue-600': etat == 'tout' }"
          >All</span
        >
        <span
          class="text-lg font-bold w-[80px] text-center"
          @click="etat = 'fait'"
          :class="{ 'border-b-4 border-blue-600': etat == 'fait' }"
          >Do</span
        >
        <span
          class="px-4 text-lg text-center font-bold"
          @click="etat = 'nofait'"
          :class="{ 'border-b-4 border-blue-600': etat == 'nofait' }"
          >Not Done</span
        >
      </div>
      <div class="overflow-scroll overflow-x-hidden">
        <div class="flex flex-col justify-center" v-if="etat == 'tout'">
          <VTasks
            class=""
            :todo="todo"
            v-for="(todo, index) in allToDo"
            :key="index"
            @clickElement="toggleTodo"
            @deleteElement="deleteTasks"
            @updateElement="updaTe"
          />
        </div>
        <div class="flex flex-col justify-center" v-if="etat == 'fait'">
          <VTasks
            class=""
            :todo="todo"
            v-for="(todo, index) in finishToDo"
            :key="index"
            @clickElement="toggleTodo"
            @deleteElement="deleteTasks"
            @updateElement="updaTe"
          />
        </div>
        <div class="flex flex-col justify-center " v-if="etat == 'nofait'">
          <VTasks
            class=""
            :todo="todo"
            v-for="(todo, index) in notToDo"
            :key="index"
            @clickElement="toggleTodo"
            @deleteElement="deleteTasks"
            @updateElement="updaTe"
          />
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, computed, onMounted } from "vue";
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

function deleteTasks(todo) {
  const index = ref("");
  index.value = todo.label;

  const label = computed(() => {
    return allToDo.value.filter((el) => el.label === index.value)[0].label;
  });

  const labelIndex = ref();
  labelIndex.value = allToDo.value.findIndex((el) => el.label === label.value);
  if (labelIndex.value === 0) {
    allToDo.value.shift();
  } else {
    allToDo.value.splice(0, labelIndex.value);
  }
  localStorage.setItem("allTodo", JSON.stringify(allToDo.value));
}

function updaTe(todo) {
  isClick.value = true;

  deleteTasks(todo);
}

function addToDo(todo) {
  if (todo.label !== "") {
    allToDo.value.push(todo);
  } else {
    alert("Le champ est vide");
  }
  localStorage.setItem("allTodo", JSON.stringify(allToDo.value));
  isClick.value = false;
}

function toggleTodo(todo) {
  if (todo.state == "progress") {
    todo.state = "finish";
  } else {
    todo.state = "progress";
  }
  localStorage.setItem("allTodo", JSON.stringify(allToDo.value));
}

onMounted(() => {
  allToDo.value = JSON.parse(localStorage.getItem("allTodo"));
  if (!allToDo.value) {
    allToDo.value = [];
  }
});
</script>

<style lang="scss" scoped></style>
