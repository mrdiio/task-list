<template>
   <div class="app">
      <table class="table">
         <thead>
            <tr>
               <td>#</td>
               <td>Task Title</td>
               <td>Priority</td>
               <td>Action</td>
            </tr>
         </thead>
         <tbody>
            <!-- <task-component></task-component> -->
            <tr v-for="(task, index) in tasks">
               <td>{{index+1}}</td>
               <td>{{task.title}}</td>
               <td>{{task.priority}}</td>
               <td><button @click="remove(task.id)" class="btn btn-danger">Remove</button></td>
            </tr>
            <tr>
               <td colspan="2">
                  <input v-model="task.title" type="text" id="task" class="form-control">
               </td>
               <td>
                  <select v-model="task.priority" name="" id="select" class="form-control">
                     <option>Low</option>
                     <option>Medium</option>
                     <option>High</option>
                  </select>
               </td>
               <td>
                  <button @click="store" class="btn btn-primary">Add</button>
               </td>
            </tr>
         </tbody>
      </table>
   </div>
</template>

<script>
import Task from './components/Task.vue';
export default {
   data() {
      return {
         tasks: [],
         task: {
            title: '',
            priority: ''
         }
      }
   },
   methods: {
      getTasks(){
         window.axios.get('api/tasks').then(({data}) => {
            data.forEach(task => {
               this.tasks.push(task)
            }); 
         });
      },
      store() {
         if (this.task.title && this.task.priority) {
            window.axios.post('api/tasks', this.task).then(savedTask => {
               this.tasks.push(savedTask.data)
               this.task.title = ''
            })
         }
         else {
            console.log('data tidak lengkap');
            
         }
      },
      remove(id) {
         window.axios.delete('api/tasks/' + id).then(() => {
            let index = this.tasks.findIndex(task => task.id === id);
            this.tasks.splice(index, 1);
         });
         console.log('Deleted ' + id);
      },
   },
   created(){
      this.getTasks();
   },
   components: {
      taskComponent: Task
   },
}
</script>

