<template>
  <header id="header">
    <button @click="editName"><i class="fa-solid fa-pen-to-square"></i></button>
    <h1>{{ user }}'s to-do list</h1>
  </header>
  <section> <input type="text" v-model="taskInput" @keypress.enter="addTaskToDoList">
  </section>
  <section>
    <h2>Tasks to-do:</h2>
    <ul>
      <li v-for="(taskToDo, index) in tasksToDo" :key="taskToDo">{{ taskToDo }}
        <button @click="moveTask(this.tasksToDo, this.finishedTasks, index)"><i class="fa-solid fa-check"></i></button>
        <button @click="deleteTask(this.tasksToDo, index)"><i class="fa-solid fa-xmark"></i></button>

      </li>
    </ul>
  </section>
  <section>
    <h2>Finished tasks:</h2>
    <ul>
      <li v-for="(finishedTask, index) in finishedTasks" :key="finishedTask">{{ finishedTask }}
        <button @click="moveTask(this.finishedTasks, this.tasksToDo, index)"><i
            class="fa-solid fa-arrow-rotate-left"></i></button>
        <button @click="deleteTask(this.finishedTasks, index)"><i class="fa-solid fa-xmark"></i></button>
      </li>
    </ul>
  </section>



</template>

<script>

export default {
  name: 'App',
  beforeMount() {
    let retrievedUserName = JSON.parse(localStorage.getItem('userName'));
    let retrievedToDoList = JSON.parse(localStorage.getItem('toDoList'));
    let retrievedFinishedList = JSON.parse(localStorage.getItem('finishedList'));

    retrievedUserName ? this.user = retrievedUserName : this.editName();
    retrievedToDoList ? this.tasksToDo = retrievedToDoList : '';
    retrievedFinishedList ? this.finishedTasks = retrievedFinishedList : '';
  },
  data() {
    return {
      user: '',
      taskInput: '',
      tasksToDo: [],
      finishedTasks: []
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

  },
}
</script>

<style>
@import url(./style.css);
</style>
