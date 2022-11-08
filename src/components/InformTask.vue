<template>
    <div>
    <div>
        <div v-if="!tasks.length" class="alert alert-info" role="alert">
      No existen tareas
    </div>
    </div>
  <div id="app" >
        <div class="card" style="width: 18rem;" v-for="task in tasks" :key="task.id" >
          <div v-if="edit === task.id">
          <input type="text" class="form-control" v-model="task.title" />
        </div>
        <div v-else>
          <h1>{{task.title}}</h1>
        </div>
          
          <div v-if="edit === task.id">
          <input type="text" class="form-control" v-model="task.description" />
        </div>
        <div  v-else>
          <p>{{task.description}}</p>
        </div>
        <div v-if="edit === task.id">
          <button class="btn btn-success" @click="saveTask(task)"> Guardar</button>
          <button class="btn btn-secondary ml-2" @click="cancelEdit(task)"> Cancelar</button>
        </div>
        <div v-else>
          <button class="btn btn-info" @click="editTask(task)"> Editar</button>
          <button class="btn btn-danger ml-2" @click="$emit('delete-task', task)"> Eliminar</button>
        </div>
        </div>
    </div>
  </div>
</template>

<script lang="ts">
export default{
  name: 'InformTask',
  props: {
   tasks: Array,

  },
  data() {
    return {
      edit: null,
  
    }
  },
  methods: {
    editTask(task) {
      this.taskEdit = Object.assign({}, task);
      this.edit = task.id;
    },
    saveTask(task) {
      if (!task.title.length || !task.description.length) {
        return;  
      }
      this.$emit('actualizar-usuario', task);
      this.edit = null;
    },
    cancelEdit(task) {
      Object.assign(task, this.taskEdit);
      this.edit = null;
    }
  }
}
</script>

<style>
.card{
  display: inline-flex;
  margin-right: 2rem;
  margin-bottom: 2rem;
}
</style>