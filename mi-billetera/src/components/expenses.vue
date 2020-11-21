<template>
  <div class='row lead my-1'>
    <div class='col-5'>{{gastos.nombre}}</div>
    <div class='col-3 text-center'>{{gastos.monto}}</div>
    <div class='col-2 text-center'>{{gastos.tipo}}</div>
    <div class='col-2 text-center'>
      <div id='editar'  class='btn btn-dark mr-1'
           v-on:click='manejarClick($event,{indice:indice, id:id})'><i class="fa fa-pencil" aria-hidden="true"></i>
      </div>
      <div id='eliminar'  class='btn btn-dark'
           v-on:click='manejarClick($event,{indice:indice, id:id})'><i class="fa fa-trash" aria-hidden="true"></i>      </div>  
    </div>
  </div>
</template>

<script>
export default {
  name: 'expenses',
  props: ['gastos','id','indice'],
  data: function () {
    return {
      test: 'test'
    }
  },
  methods: {
    manejarClick: function (evento,gastoID) {
      if (evento.target.id==='eliminar'){
        this.$emit('eliminarGasto',gastoID)
      }
      else if (evento.target.id==='editar'){
        console.log("editar")
      }
    }
  }
}
</script>

<style>

</style>
