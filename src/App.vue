<template>
  <header>
    <button @click="editName"><i class="fa-solid fa-pen-to-square fa-m"></i></button>
    <h1 class="d-inlineBlock half-marginLeft">{{ user }}'s to-do list</h1>
  </header>
  <section> <input class="w-80 bg-glass" type="text" v-model="taskInput" @keypress.enter="addTaskToDoList"
      placeholder="Add your task...">
    <button id="listChangerBtn" class="d-flex-center w-80" @click="changeList">
      <span :class="[listDisplay ? listSelectorBtnClass : '']">Tasks to-do</span>
      <span :class="[!listDisplay ? listSelectorBtnClass : '']">Finished Tasks</span></button>
  </section>
  <section v-if="listDisplay" class="list-section">
    <ul>
      <li v-for="(taskToDo, index) in tasksToDo" :key="taskToDo" class="d-flex-center w-80 bg-glass">
        <span>{{ taskToDo }}</span>
        <div class="btn-pos half-marginLeft">
          <button class="btn btn-green" @click="moveTask(this.tasksToDo, this.finishedTasks, index)">
            <i class="fa-solid fa-check fa-xs"></i></button>
          <button class="btn half-marginLeft btn-red" @click="deleteTask(this.tasksToDo, index)">
            <i class="fa-solid fa-xmark fa-xs"></i></button>
        </div>
      </li>
    </ul>
  </section>
  <section v-if="!listDisplay" class="list-section">
    <ul>
      <li v-for="(finishedTask, index) in finishedTasks" :key="finishedTask" class="d-flex-center w-80 bg-glass">
        <span>{{ finishedTask }}</span>
        <div class="btn-pos half-marginLeft">
          <button class="btn btn-blue" @click="moveTask(this.finishedTasks, this.tasksToDo, index)">
            <i class="fa-solid fa-arrow-rotate-left fa-xs"></i></button>
          <button class="btn half-marginLeft btn-red" @click="deleteTask(this.finishedTasks, index)">
            <i class="fa-solid fa-xmark fa-xs"></i></button>
        </div>
      </li>
    </ul>
  </section>



</template>

<script>

export default {
  name: 'App',
  beforeMount() {
    const retrievedUserName = JSON.parse(localStorage.getItem('userName'));
    const retrievedToDoList = JSON.parse(localStorage.getItem('toDoList'));
    const retrievedFinishedList = JSON.parse(localStorage.getItem('finishedList'));

    retrievedUserName ? this.user = retrievedUserName : this.editName();
    retrievedToDoList ? this.tasksToDo = retrievedToDoList : '';
    retrievedFinishedList ? this.finishedTasks = retrievedFinishedList : '';
  },
  data() {
    return {
      user: '',
      taskInput: '',
      tasksToDo: [],
      finishedTasks: [],
      listDisplay: true,
      listSelectorBtnClass: 'selected-list'
    }
  },
  watch: {
    tasksToDo: {
      handler() {
        localStorage.setItem('toDoList', JSON.stringify(this.tasksToDo));
      },
      deep: true,
    },
    finishedTasks: {
      handler() {
        localStorage.setItem('finishedList', JSON.stringify(this.finishedTasks));
      },
      deep: true,
    },
  },
  methods: {
    editName() {
      this.user = prompt('Type your name...');
      localStorage.setItem('userName', JSON.stringify(this.user));
    },
    deleteTask(array, index) {
      array.splice(index, 1);
    },
    addTaskToDoList() {
      this.tasksToDo.push(this.taskInput);
      this.taskInput = "";
    },
    moveTask(fromArray, toArray, index) {
      toArray.push(fromArray[index]);
      this.deleteTask(fromArray, index);
    },
    changeList() {
      this.listDisplay = !this.listDisplay;
    }
  },
}
</script>

<style>
@import url(./style.css);
</style>
