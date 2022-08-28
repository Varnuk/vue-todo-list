<template>
<li class="single-task" :class="{ completed: isCompleted}" >{{!isEditing?item.task:''}}

      <div v-if="!isEditing" class="control-tools">
        <button class="task-accept" @click="()=>isCompleted=!isCompleted"><span>&#10004;</span></button>
        <button class="task-edit" @click="()=>isEditing=!isEditing"><span>&#9998;</span></button>
        <button class="task-remove" @click="$emit('removeTaskEvent',item.id)"><span>&#10006;</span></button>
      </div>
      
      <input v-if="isEditing" v-model="editedTask"  type="text">
      <button 
      v-if="isEditing" 
      class="task-accept" 
      @click="()=>{
        if(editedTask==='')
          editedTask=item.task;
        
        editedTask=editedTask.toUpperCase();
        $emit('editTaskEvent', item.id, editedTask); 
        isEditing=!isEditing;}">
        <span>&#10004;</span>
        </button>
</li>
</template>

<script>
export default {
  name: 'SingleTask',
  props: {
    item: Object,
  },
  data(){
    return{
        isCompleted: false,
        isEditing:false,
        editedTask: this.item.task
    }
  }
}
</script>

<style scoped>
button{
  border: none;
  background-color: transparent;
  cursor: pointer;
  transition: background-color 0.2s ease;
  padding: 15px 10px;
  display: inline-block;
}

button:hover{
  background-color: #ddd;
}

.task-accept{
  color: rgb(15, 179, 69);
}

.task-edit{
  color: rgb(0, 132, 255);
}

.task-remove{
  color: orangered;
}

.completed{
  text-decoration: line-through;
  background-color: rgb(123, 249, 149);
}

.single-task{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li{
  padding-left: 20px;
  margin: 10px;
  background-color: rgb(245, 245, 245);
}
.control-tools{
  background-color: rgb(245, 245, 245);
}

input{
  margin: 0;
  
}

</style>
