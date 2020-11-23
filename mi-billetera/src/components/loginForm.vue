<template>
    <div id='login'>
      <div class='container my-3 border rounded' style='width:350px'>
        <div class='row'>
          <div class='col-12 text-center mt-3 mb-3'><h3>Ingreso al Sistema</h3></div>
        </div>
        <div class="form-group">
            <label for="email" class="cols-sm-2 control-label">Correo Electrónico</label>
            <div class="cols-sm-10">
              <div class="input-group">
                <span class="pt-2 px-3 input-group-addon bg-info">
                  <i class="fa fa-envelope fa" aria-hidden="true"></i>
                </span>
                <input v-model='correo' type="text" class="form-control" placeholder='Ingrese el Correo Electrónico'/>
              </div>
            </div>
          </div>
        <div class="form-group">
            <label for="password" class="cols-sm-2 control-label">Contraseña</label>
            <div class="cols-sm-10">
              <div class="input-group">
                <span class="pt-2 px-3 input-group-addon bg-info">
                  <i class="fa fa-lock fa-lg" aria-hidden="true"></i>
                </span>
                <input type="password" class="form-control" placeholder='Ingrese la contraseña' v-model='clave' />
              </div>
            </div>
          </div>
        <div class="form-group row mt-1 mb-1 d-flex justify-content-center">
            <button type="button" class="btn btn-dark" v-on:click="ingresar">Ingresar</button>
        </div>
        <div id="error">
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name:'loginForm',
  data: function () {
    return {
      correo:'',
      clave:'',
      login:true
    }
  },
  props: ['firebase'],
  methods: {
    ingresar: function() {
      this.firebase.auth().signInWithEmailAndPassword(this.correo, this.clave)
      .then((response) => {
        console.log('Ingreso correcto con User: '+response.user.email)
        this.$emit('ingresoCorrecto',response.user.email)
      })
      .catch((error) => {
        var errorCode = error.code;
        var errorMessage = error.message;
        this.correo =  ""
        this.clave =""
        document.getElementById("error").innerText ="Usuario o Contraseña invalidos"
       console.log('Error: '+errorCode+' - '+errorMessage);
      })
    }
  }
}
</script>

<style>
</style>
