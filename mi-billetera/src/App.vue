<template>
  <div id="app" class='container'>
    <header>
      <div class="row mt-4 mb-4"> 
        <div class="col-11 text-center">
          <h1>Lista de Gastos</h1>
        </div>
        <div class="col-1 text-center aling-center">
          <div class="btn btn-dark mt-2" @click="toggleShowComplete">
            <span v-if="!showComplete">+</span>
            <span v-else>...</span>
          </div>
        </div>
      </div>
    </header>
    <div v-if="showComplete" class="border rounded" id="menu">
      <div class="col-5 container justify-content-center">
        <div class="row mt-1">
          <div class="col-5">
            Nombre del gasto
          </div>
          <div class='col-2 '>
            <input type='text'>
          </div>
        </div>
        <div class="row mt-1">
          <div class="col-5">
            Monto del gasto
          </div>
          <div class='col-2'>
            <input type='text'>
          </div>
        </div>
        <div class="row mt-1">
          <div class="col-5">
            Tipo de gasto
          </div>
          <div class='col-2'>
            <input type='text'>
          </div>
        </div>
        <div class="row mt-1 mb-1 d-flex justify-content-center">
          <div class="btn btn-dark" @click="toggleShowComplete">
            Agregar
          </div>
        </div>
      </div>  
    </div>
  </div>
</template>

<script>

 import firebase from "firebase/app"
 import "firebase/firestore"
 import '../node_modules/bootstrap/dist/css/bootstrap.css'
 import '../node_modules/font-awesome/css/font-awesome.css' 
 import "./App.css"

export default {
  name: 'app',
  data: function (){
    return {
      showComplete: false
    }
  },
  methods:{
    toggleShowComplete() {
      this.showComplete = !this.showComplete;
    },
  }
}
</script>

<style>
input{
    width: 15rem;
  }
</style>