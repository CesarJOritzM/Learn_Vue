<template>
<div id="app">
    <section class="todo-wrapper">
      <h1 class="todo-title">Lista de tareas</h1>
      <form @keydown.enter.prevent>
        <input type="text" class="input-todo" 
               v-bind:class="{ active: new_todo }" placeholder="Indica la tarea que debes realizar" 
               v-model="new_todo" v-on:keyup.enter="addItem">
        <div class="btn btn-add" v-bind:class="{ active: new_todo }"  @click="addItem">+</div>
      </form>

      <div v-if="pending.length > 0">
        <p class="status busy">Tienes {{ pending.length }} tareas pendiente<span v-if="todoList.length>1">s</span></p>
        <transition-group name="todo-item" tag="ul" class="todo-list">
          <Tarea v-for="(item) in pending" 
             v-bind:key="item.title" 
             v-bind:item='item'
             v-on:delete='deleteItem($event)'></Tarea>
        </transition-group>  
      </div> 
      <transition name="slide-fade">
        <p class="status free" v-if="!pending.length" >
          Tiempo de celebrar, no tienes tareas pendientes</p> 
      </transition> 

      <div v-if="completed.length > 0 && showComplete">
        <p class="status">Tareas completadas: {{ completedPercentage }}</p>
        <transition-group name="todo-item" tag="ul" class="todo-list archived">
          <Tarea v-for="(item) in completed" 
             v-bind:key="item.title" 
             v-bind:item='item'
             v-on:delete='deleteItem($event)'></Tarea>
        </transition-group>  
      </div>  
      <div class="control-buttons">
        <div class="btn btn-secondary" 
             v-if="completed.length > 0" @click="toggleShowComplete">
             <span v-if="!showComplete">Mostrar</span>
             <span v-else>Ocultar</span> Completadas</div>
        <div class="btn btn-secondary" 
              v-if="todoList.length > 0" @click="clearAll">Borrar todas</div>
      </div>
    </section>
  </div>
</template>


<script>
import Tarea from './components/Tarea.vue'
import "./App.css"

export default {
  name: 'app',
  data: function (){
    return {
      todoList: [
        {"id":0,"title":"Estudiar Vue","done":false},
        {"id":1,"title":"Practicas desarrollando con Vue","done":false},
        {"id":4,"title":"Hacer curso de NextU","done":true}
      ],
      new_todo:'',
      showComplete: false
    }
  },
  components: {
    Tarea
  },
  computed: {
    pending: function() {
      return this.todoList.filter(function(item) {
        return !item.done;
      })
    },
    completed: function() {
      return this.todoList.filter(function(item) {
        return item.done;
      }); 
    },
    completedPercentage: function() {
      return (Math.floor((this.completed.length / this.todoList.length) * 100)) + "%";
    }
  },
  methods:{
    toggleShowComplete() {
      this.showComplete = !this.showComplete;
    },
    addItem() {
      if (this.new_todo) {
        this.todoList.unshift({
          id: this.todoList.length,
          title: this.new_todo,
          done: false,
        });
      }
      this.new_todo = '';
      return true;
    },
    deleteItem(item) {
      this.todoList.splice(this.todoList.indexOf(item), 1);
    },
    clearAll() {
      this.todoList = [];
    }
  }
}
</script>

