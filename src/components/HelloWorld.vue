<template>
  <body>
    <div id="principal">
      <div>
            <InformTask :tasks="tasks" @eliminar-task="deleteTask"/>
      </div>
      <div>
        <InsertTask @crear-task="postTask"/>
      </div>
     
    </div>
   
</body>

</template>


<script lang="ts">

import InformTask from '../components/InformTask.vue'
import InsertTask from './InsertTask.vue';
export default {
  components: {
    InformTask,
    InsertTask
  },
  name: 'HelloWorld',
  data() {
    return {
      tasks: Array,
      task:{
      id: Number,
      title: "",
      description: ""
      }
    
    }
   
  },
   methods: {
      async getTasks() {
        try {
       const response = await fetch('http://localhost:8091/api/task/get-all');
       this.tasks = await response.json();
       } catch (error) {
        console.error(error);
        }
      },
      async postTask(task: any) {
        try {
    const response = await fetch('http://localhost:8091/api/task/post', {
      method: 'POST',
      body: JSON.stringify(task),
      headers: { 'Content-type': 'application/json; charset=UTF-8' },
    });
    
    const taskCreado = await response.json();
    this.tasks = [...this.tasks, taskCreado];
  } catch (error) {
    console.error(error);
  }
      },

     async putTask(task: { id: any; }) {
        try {
    const response = await fetch(`http://localhost:8091/api/task/update/${task.id}`, {
      method: 'PUT',
      body: JSON.stringify(task),
      headers: { 'Content-type': 'application/json; charset=UTF-8' },
    });
    
    const taskActualizado = await response.json();
    this.tasks = this.tasks.map(t => (t.id === task.id ? taskActualizado : t));
  } catch (error) {
    console.error(error);
  }
      },
      async deleteTask(task) {
        try {
       await fetch(`http://localhost:8091/api/task/delete/${task.id}`, {
      method: "DELETE"
    });
    
    this.tasks= this.tasks.filter(t => t.id !== task.id);
  } catch (error) {
    console.error(error);
  }
      },
    },
    mounted() {
      this.getTasks();
      
    }
}
</script>




<style scoped>


#principal{
 display: flex;
 flex-direction:column-reverse;
}
</style>
