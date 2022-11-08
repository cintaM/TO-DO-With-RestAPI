<template>
  <div id="form-task" class="card">
  <form @submit.prevent="addForm">
    <div class="container">
      <div class="row">
        <div class="col-md-4">
          <div class="form-group">
            <label>Titulo</label>
            <input
              placeholder="introduce un título"
              ref="title"
              v-model="task.title"
              type="text"
              class="form-control"
              :class="{ 'is-invalid': loading && titleNotValid }"
              @focus="resetStatus"
              @keypress="resetStatus"
            />
          </div>
        </div>
        <div class="col-md-4">
          <div class="form-group">
            <label> Descripción</label>
            <input
            placeholder="introduce una descripción"
              v-model="task.description"
              type="text"
              :class="{ 'is-invalid': loading && descriptionNotValid }"
              class="form-control"
              @focus="resetStatus"
            />
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-4">
          <div class="form-group">
            <button class="btn btn-primary">Añadir Tarea</button>
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <div v-if="error && loading" class="alert alert-danger" role="alert">
            Debes rellena todos los campos.
          </div>
          <div v-if="allOk" class="alert alert-success" role="alert">
            La tarea ha sido agregada!
          </div>
        </div>
      </div>
    </div>
  </form>
</div>
</template>

<script lang="ts">
export default {
  name: 'InsertTask',
  data() {
    return {
      loading: false,
      allOk: false,
      error: false,
      task: {
        title: '',
        description: '',
      }
    }
  },
  methods: {
    addForm() {
      this.loading = true;
      this.resetStatus();
            
      // Comprobamos la presencia de errores
      if (this.titleNotValid|| this.descriptionNotValid) {
        this.error = true;
        return;
      }
      
      this.$emit('crear-task', this.task);
      this.error = false;
      this.allOk = true;
      this.loading = false;
      
      // Restablecemos el valor de la variables
      this.task= {
        title: '',
        description: '',
      }
    },
    resetStatus() {
      this.allOk = false;
      this.error = false;
    }
  },
  computed: {
    titleNotValid() {
      return this.task.title.length < 1;
    },
    descriptionNotValid() {
      return this.task.description.length < 1;
    },
  },
}
</script>

<style>
.row{
    display: flex;
    flex-direction: column;
}
.form-control{
    width: 15rem;
}

button{
    width: 15rem;
    margin-top: 1rem;
}

</style>