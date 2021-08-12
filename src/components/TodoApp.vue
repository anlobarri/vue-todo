<template>
  <div class="container">
    <h1 class="text-center mt-5 mb-5">Vue Todo App</h1>

    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Añade una nueva Tarea" class="form-control" @keyup.enter="submitTask">
      <button @click="submitTask" class="btn btn-warning rounder-0 btn-add">Añadir</button>
    </div>
    
    <table class="table table-bordered mt-5">
      <thead>
        <tr class="text-center">
          <th scope="col">Tarea</th>
          <th scope="col">Estado</th>
          <th scope="col" class="text-center">Editar</th>
          <th scope="col" class="text-center">Borrar</th>
        </tr>
      </thead>
      <tbody>
        <tr 
        v-for="(task, index) in tasks"
        :key="index">
          <td :class= "{'finished': task.status === 'Hecho'}">
            {{ task.name }}
          </td>
          <td class="text-center" style="width:120px">
            <span @click="changeStatus(index)" class="pointer" 
            :class="{'text-danger': task.status == 'ToDo',
            'text-warning': task.status == 'Haciendo',
            'text-success': task.status == 'Hecho'
            }">
              {{ task.status}}
            </span>
          </td>
          <td style="width:90px">
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen pointer"></span>
            </div>
          </td>
          <td style="width:90px">
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
        </tr>        
      </tbody>
    </table>


  </div>
</template>

<script>
export default {
  name:'HelloWorld',
  props:{
    msg:String
  },
  data(){
    return{
      task:'',
      editedTask: null,
      availableStatuses:['ToDo', 'Haciendo', 'Hecho'],
      tasks:[
        {name: 'Aprender VueJs', status:'Haciendo'},
        {name: 'Mirar al cielo', status:'ToDo'},
      ]
    }
  },
  methods: {
    submitTask(){
      if(this.tasks === 0) return;

      if(this.editedTask === null){
        this.tasks.unshift({
          name:this.task,
          status:'ToDo'
        }); 
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = ''
    },

    deleteTask(index){
      this.tasks.splice(index, 1);
    },
    
    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
  },
}
</script>


<style scoped>
  .pointer{
    cursor: pointer;
  }

  .finished{
    text-decoration: line-through;
  }

  .btn-add{
    margin-left: 5px;;
  }

  
</style>