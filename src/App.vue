<template>
  <div id="#app">
    <img alt="Vue logo" src="./assets/logo.png">

    <div class="container">
      <div class="container--header">
      <h1>TODO LIST</h1>
        <input v-model="newTask" type="text" placeholder="Add a new task..."/>
        <button @click="addNewTask" class="btn-add">ADD</button>
      </div>

      <div v-if="todoList.length" class="task-list">
        <h2 v-if="todoList.length">Your tasks!</h2>
        <ul class="container--task">
          <SingleTask 
          v-for="singleTask in todoList" 
          @remove-task-event="removeTask" 
          @edit-task-event="editTask" 
          :key="singleTask.id" 
          :item="singleTask"/>
        </ul>
      </div>

    </div>
  </div>
</template>

<script>
import SingleTask from './components/SingleTask.vue'
import { uuid } from 'vue-uuid'; 

export default {
  name: "App",
  data() {
    return {
      newTask: '',
      todoId: '',
      todoList:[]
    };
  },
  methods:{
    addNewTask(){
      if(this.newTask==="")
        return;
      const task = {
        id: uuid.v4(),
        task: this.newTask.toUpperCase()
      }
      this.todoList.push(task);
      this.newTask='';
    },

    removeTask(taskId){
      this.todoList = this.todoList.filter(task => task.id !== taskId);
    },

    editTask(taskId, newTaskName){
      const newArray=[];

      for(let i = 0; i < this.todoList.length; i++) {
        const oldTask = this.todoList[i];
        if(oldTask.id === taskId) {
          newArray.unshift({ ...oldTask, task: newTaskName })
        } else {
          newArray.push(oldTask)
        }
      }
      this.todoList=newArray;
    }
  },
  components: {
    SingleTask
  }
  }
</script>

<style>
body{
  background-color: var(--light);
  font-family: 'Work Sans', sans-serif;
}

#app {
  text-align: center;
  margin-top: 60px;
}

:root {
  --deep-dark: rgb(26, 26, 26);
  --light-dark: rgb(79, 82, 92);
  --light: rgb(245, 245, 245);
}

.container{
  background-image: linear-gradient(340deg, #045de9 0%, #09c6f9 100%);
  width: 800px;
  margin: auto;
  border-radius: 10px;
  color: var(--light);
  padding: 0;
  box-shadow: 4px 4px 5px rgba(82, 82, 82, 0.4);
}

.task-list{
  background-color: white;  
  text-transform: uppercase;
  margin-bottom: 16px;
}

.container--header{
  padding: 20px;
  font-size: 0.8rem;
}

input{
  border: none;
  margin-right: 20px;
  margin-bottom: 10px;
  width: 80%;
  padding: 10px;
  border-radius: 5px;
}

.btn-add{
  border: none;
  padding: 10px 15px;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.2s ease;
}

.btn-add:hover {
  color: #fff;
  background-color: var(--light-dark);
}

.task-list{
  color: var(--deep-dark);
  font-size: 0.8rem;
  padding: 10px;
}

img{
  width: 70px;
  margin-bottom: 20px;
}

ul{
    list-style: none;
    margin: 0;
    padding: 0;
}

</style>
