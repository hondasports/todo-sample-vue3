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
  <div>TODOリスト</div>
  <div v-if="isEmptyTodos">
    タスクがない。
  </div>
  <ul v-else>
    <li v-for="(todo, index) in todos" :key="index">
      <div class="todo-container">
        <input type="checkbox" v-model="todos[index].completed">
        <span class="todo-item">
          <span @click="enableTodoView(index)">
            <input type="text" v-model="todos[index].message" :disabled="! todoViewState[index].isEditable">
          </span>
          <button @click="deleteTodo(index)">削除</button>
          <button v-if="todoViewState[index].isEditable" @click="disableTodoView(index)">確定</button>
          <span>{{ todos[index].completed ? '完了済み' : '' }}</span>
        </span>
      </div>
    </li>
  </ul>
  <div class="operation-container">
    新規タスク: <input type="text" v-model="taskContent"><button @click="addTodo" :disabled="! taskContent.length">追加</button>
  </div>
</template>

<style scoped>
</style>
