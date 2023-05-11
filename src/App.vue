<script setup lang="ts">
import { computed, reactive, ref } from 'vue';

interface TodoViewState {
  isEditable: boolean
}

interface Todo {
  id: number
  message: string
  completed: false
}

const todos = reactive<Todo[]>([]);

const todoViewState = reactive<TodoViewState[]>([]);
const taskContent = ref('');

const addTodo = () => {
  todos.push({
    id: todos.length,
    message: taskContent.value,
    completed: false
  });

  todoViewState.push({isEditable: false});

  taskContent.value = '';
};

const deleteTodo = (index: number) => {
  todos.splice(index, 1);
  todoViewState.splice(index, 1);
};

const enableTodoView = (index: number) => {
  if (todos[index].completed) {
    return;
  }

  todoViewState[index].isEditable = true;
};

const disableTodoView = (index: number) => {
  todoViewState[index].isEditable = false;
};

const isEmptyTodos = computed(() => {
  return todos.length === 0;
});

</script>

//  * 目的
//    * TODOアプリの作成を通じてvue3とtypescript使い方を学ぶ。
//  * 機能
//    * [x] タスクの新規追加
//    * [x] タスクの削除
//    * [x] タスクの編集
//    * タスク一覧表示
//      * [x] 一覧は、リスト形式で表示する。
//      * [x] タスクが存在しない時は、タスクが存在しないことを表示する。
// TODO:
//  * デザイン。
//    * tailwindの使い方を。

<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-2xl font-bold mb-6">TODOリスト</h1>
    <div v-if="isEmptyTodos" class="text-center py-6">
      タスクがない。
    </div>
    <ul v-else>
      <li v-for="(todo, index) in todos" :key="index" class="mb-4">
        <div class="flex items-center space-x-4">
          <input type="checkbox" v-model="todos[index].completed" class="rounded">
          <span class="flex-1" @click="enableTodoView(index)">
            <input type="text"
              class="w-full px-4 py-2 rounded border border-gray-300"
              :class="{ 'line-through': todos[index].completed }"
              v-model="todos[index].message"
              :disabled="! todoViewState[index].isEditable" >
          </span>
          <button @click="deleteTodo(index)" class="px-4 py-2 bg-red-500 text-white rounded">削除</button>
          <button v-if="todoViewState[index].isEditable" @click="disableTodoView(index)" class="px-4 py-2 bg-green-500 text-white rounded">確定</button>
          <span>{{ todos[index].completed ? '完了' : '' }}</span>
        </div>
      </li>
    </ul>
    <div class="flex items-center space-x-4">
      <label for="task-input" class="text-gray-700">新規タスク:</label>
      <input id="task-input" type="text" v-model="taskContent" class="flex-1 px-4 py-2 rounded border border-gray-300">
      <button @click="addTodo" :disabled="! taskContent.length" class="px-4 py-2 bg-blue-500 text-white rounded">追加</button>
    </div>
  </div>
</template>

<style scoped>
</style>
