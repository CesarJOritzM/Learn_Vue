<template>
  <div v-if="logon" id="app" class='container'>
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
    <div id="menu" v-if="showComplete" class="border rounded" >
      <div class="col-5 container justify-content-center">
        <div class="row mt-1">
          <div class="col-5">
            Nombre del gasto
          </div>
          <div class='col-2 '>
            <input v-model="nombreGasto" type='text'>
          </div>
        </div>
        <div class="row mt-1">
          <div class="col-5">
            Tipo de gasto
          </div>
          <div class='col-2'>
            <input v-model="tipoGasto" type='text'>
          </div>
        </div>
        <div class="row mt-1">
          <div class="col-5">
            Monto del gasto
          </div>
          <div class='col-2'>
            <input v-model="montoGasto" type='text'>
          </div>
        </div>
        <div class="row mt-1 mb-1 d-flex justify-content-center">
          <div id="agregar" class="btn btn-dark" v-on:click="manejarClick($event)">
            Agregar
          </div>
        </div>
      </div>  
    </div>
    <div id="content" class='container border rounded text-dark mt-3'>
    <div class='row lead border-bottom rounded font-weight-bold'>
      <div class='col-5 '>
        Nombre del Gasto
      </div>
      <div class='col-3 text-center'>
        Monto del Gasto
      </div>
      <div class='col-2 text-center'>
        Tipo de Gasto
      </div>
      <div class='col-2 text-center'>
        Accion
      </div>
    </div>
    <expenses
      v-for="(gastos,index) in gastos"
          v-bind:gastos='gastos'
          v-bind:id='gastos.id'
          v-bind:indice='index'
          v-bind:key='index'
          v-on:eliminarGasto='eliminar($event)'>
    </expenses>
    <div class='row lead border-top rounded font-weight-bold'>
      <div class='col-6 text-center'>
        TOTAL
      </div>
      <div class='col-6 text-left'>
        suma
      </div>
    </div>
  </div>
  </div>
  <div v-else>
    <loginForm v-on:ingresoCorrecto='ingresoCorrecto($event)'></loginForm>
  </div>
</template>

<script>

//  import firebase from 'firebase'
//  import 'firebase/firestore'
 import '../node_modules/bootstrap/dist/css/bootstrap.css'
 import '../node_modules/font-awesome/css/font-awesome.css' 
 import loginForm from "./components/loginForm.vue"
 import expenses from "./components/expenses.vue"
 
export default {
  name: 'app',
  data: function (){
    return {
      gastos:[{nombre:'Netflix',monto:79400, tipo:"Hogar"},
              {nombre:'Ruta',monto:78000, tipo:"Trabajo"},
              {nombre:'Gas',monto:63000, tipo:"Hogar"}],
      nombreGasto:"",
      tipoGasto:"",
      montoGasto:"",
      showComplete: false,
      logon: true,
      // coleccion:{},
      // firebase:'',
      // idUsuario:'',
      // db:''
    }
  },
  methods:{
    toggleShowComplete() {
      this.showComplete = !this.showComplete;
    },
    manejarClick: function (evento)  {
      if (evento.target.id==='agregar'){
        // const gastoData = {nombre:this.nombreGasto,
        //                   monto:this.montoGasto,
        //                   tipo:this.tipoGasto}
        this.gastos.push({nombre:this.nombreGasto, monto:this.montoGasto, tipo:this.tipoGasto})
        // this.coleccion.add(gastoData)
        // .then((docReference) => {
        //   this.gastos.unshift({id:docReference.id, nombre: gastoData.nombre, monto: gastoData.monto, tipo:gastoData.tipo})
        // })
        // .catch((Errro) => {
        //   alert('No se pudo agregar el libro al sistema. Error: '+Errro.message)
        // })
        this.nombreGasto=''
        this.tipoGasto=''
        this.montoGasto=''
      }
    }, 
    ingresoCorrecto: function(usuario) {
        console.log('User: '+usuario)
        this.idUsuario=usuario
        this.logon=true
      //   this.coleccion = this.db.collection('/usuarios/'+usuario+'/libros')
      //   this.coleccion.get()
      //   .then((libros)=>{
      //     libros.forEach((libro) => {
      //       this.libros.push({id:libro.id,nombre:libro.data().nombre,autor:libro.data().autor})
      //   })
      // })
    },
    eliminar: function (gastoID){
      // this.coleccion.doc(gastoID.id).delete()
      this.gastos.splice(gastoID.indice,1)
    }
  },
  components:{
    loginForm,
    expenses
  },
  // beforeMount: function () {
  //   var config = {
  //     apiKey: "AIzaSyBh2WLh1cv9_l_5juyFadNU4RoI1Ba3QR8",
  //     authDomain: "vue-datastore.firebaseapp.com",
  //     databaseURL: "https://vue-datastore.firebaseio.com",
  //     projectId: "vue-datastore",
  //     storageBucket: "vue-datastore.appspot.com",
  //     messagingSenderId: "9302196982"
  //   };
  //   firebase.initializeApp(config);
  //   this.db = firebase.firestore();
  //   const settings = {timestampsInSnapshots: true};
  //   this.db.settings(settings);
  //   this.firebase = firebase
  //   console.log(this.firebase);
  // }
}
</script>

<style>
input{
    width: 15rem;
  }
</style>