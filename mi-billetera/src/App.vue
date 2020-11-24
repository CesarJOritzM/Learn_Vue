<template>
  <div v-if="logon" id="app" class='container'>
    <header>
      <div class="row mt-4 mb-4"> 
        <div class="col-11 text-center">
          <h1>Lista de Gastos</h1>
        </div>
        <div class="col-1 text-center aling-center">
          <div id="mostrar" class="btn btn-dark mt-2" @click="toggleShowComplete">
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
            Monto del gasto
          </div>
          <div class='col-2'>
            <input v-model="montoGasto" type='number'>
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
        <div class="row mt-1 mb-1 d-flex justify-content-center">
          <div v-if="!showEdit" id="agregar" class="btn btn-dark" v-on:click="manejarClick($event)">
            Agregar
          </div>
          <div v-else id="actualizar" class="btn btn-dark" v-on:click="toggleShowComplete(manejarClick($event))">
            Actualizar
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
      v-for="(Gastos,index) in Gastos"
          v-bind:Gastos='Gastos'
          v-bind:id='Gastos.id'
          v-bind:indice='index'
          v-bind:key='index'
          v-on:eliminarGasto='eliminar($event)'
          v-on:editarGastos='toggleShowEdit(editar($event))'>
    </expenses>
    <div class='row lead border-top rounded font-weight-bold'>
      <div class='col-5 text-center'>
        TOTAL
      </div>
      <div v-bind:text="montoTotal" class='col-3 text-center'>
        {{montoTotal}}
      </div>
    </div>
  </div>
  </div>
  <div v-else>
    <loginForm 
      v-bind:firebase="firebase"
      v-on:ingresoCorrecto='ingresoCorrecto($event)'>
    </loginForm>
  </div>
</template>

<script>

 import firebase from 'firebase'
 import 'firebase/firestore'
 import '../node_modules/bootstrap/dist/css/bootstrap.css'
 import '../node_modules/font-awesome/css/font-awesome.css' 
 import loginForm from "./components/loginForm.vue"
 import expenses from "./components/expenses.vue"

function MaysPrimera(string){
  return string.charAt(0).toUpperCase() + string.slice(1);
}

export default {
  name: 'app',
  data: function (){
    return {
      Gastos:[],
      nombreGasto:"",
      tipoGasto:"",
      montoGasto:"",
      id:{},
      showComplete: false,
      showEdit:false,
      logon: false,
      montoTotal:"" ,
      coleccion:{}, 
      firebase:'',
      idUsuario:'',
      db:''
    }
  },
  methods:{
    toggleShowComplete() {
      this.showComplete = !this.showComplete;
      this.showEdit=false
      this.nombreGasto=''
      this.tipoGasto=''
      this.montoGasto= ''
      this.id=''
    },
    toggleShowEdit() {
      this.showEdit = true
      this.showComplete = true
    },
    manejarClick: function (evento)  {
      if (evento.target.id==='agregar'){
        const gastoData = {nombre:MaysPrimera(this.nombreGasto),
                          monto:this.montoGasto,
                          tipo:MaysPrimera(this.tipoGasto)}
        this.coleccion.add(gastoData)
        .then((docReference) => {
          this.Gastos.unshift({id:docReference.id, nombre:gastoData.nombre, tipo:gastoData.tipo, monto: parseFloat(gastoData.monto),})
        })
        .catch((Error) => {
          alert('No se pudo agregar el Gastos al sistema. Error: '+Error.message)
        })
        this.nombreGasto=''
        this.tipoGasto=''
        this.montoGasto=''
      }
      else if (evento.target.id==='actualizar'){
        if (this.Gastos[this.id.indice].id === this.id.id){
          const actData = {nombre:MaysPrimera(this.nombreGasto),
                           monto:this.montoGasto,
                           tipo:MaysPrimera(this.tipoGasto)}
          this.Gastos[this.id.indice].nombre = actData.nombre;
          this.Gastos[this.id.indice].monto = parseFloat(actData.monto);
          this.Gastos[this.id.indice].tipo = actData.tipo;
          this.coleccion.doc(this.id.id).update({
            monto:this.montoGasto,
            nombre:this.nombreGasto,
            tipo:this.tipoGasto
         }) 
         .then(function() {
              console.log("Updated");
          })
         .catch((Error) => {
          alert('No se pudo agregar el Gastos al sistema. Error: '+Error.message)
        })
          this.nombreGasto=''
          this.tipoGasto=''
          this.montoGasto= ''
          this.id=''
        }
      }   
    }, 
    ingresoCorrecto: function(usuario) {
        console.log(  'User: '+usuario)
        this.idUsuario=usuario
        this.logon=true
        this.coleccion = this.db.collection('/Usuarios/'+this.idUsuario+'/Gastos/')
         this.coleccion.get()
         .then((Gastos)=>{
          Gastos.forEach((gasto) => {
            this.Gastos.push({id:gasto.id, nombre:gasto.data().nombre, monto:parseFloat(gasto.data().monto), tipo:gasto.data().tipo })
        })
      })
    },
    eliminar: function (gastoID){
      this.coleccion.doc(gastoID.id).delete()
      this.Gastos.splice(gastoID.indice,1)
    },
    editar: function (gastoID) {
      this.nombreGasto = this.Gastos[gastoID.indice].nombre
      this.tipoGasto = this.Gastos[gastoID.indice].tipo
      this.montoGasto = this.Gastos[gastoID.indice].monto
      this.id = gastoID
      }
  },
  components:{
    loginForm,
    expenses
  },
  beforeMount: function () {
    var config = {
      apiKey: "AIzaSyAE_Lftbp4CszTXT-kxkO2SYW95VSTXQWM",
      authDomain: "mi-billeterafinal.firebaseapp.com",
      databaseURL: "https://mi-billeterafinal.firebaseio.com",
      projectId: "mi-billeterafinal",
      storageBucket: "mi-billeterafinal.appspot.com",
      messagingSenderId: "1051310966835",
    };
    firebase.initializeApp(config);
    this.db = firebase.firestore();
    const settings = {timestampsInSnapshots: true};
    this.db.settings(settings);
    this.firebase = firebase;
  },
  updated:function(){
    var suma = this.Gastos.reduce((sum, value) => ( sum + value.monto ), 0);
    this.montoTotal = Intl.NumberFormat().format(suma);
  }
  
}
</script>

<style>
input{
    width: 15rem;
  }
</style>