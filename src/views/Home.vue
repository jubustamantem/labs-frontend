<template>
  <div class="container-fluid mt-2">
    <h1>Bienvenido. Seleccione una de las opciones a continuación:</h1>
    <div class="row mx-auto mt-2">
      <router-link :to="{path:'/mis-cursos'}"><button>Ver mis cursos</button></router-link>
    </div>
    <div class="row mx-auto mt-2">
      <router-link :to="{path:'/principal/nuevo-rol'}"><button>Agregar rol</button></router-link>
    </div>
    <div class="row mx-auto mt-2">
      <router-link :to="{path:'/principal/roles'}"><button>Ver roles</button></router-link>
    </div>
    <div class="row">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
  import {getAuthenticationToken} from '@/dataStorage';

  export default{
    name: "Home",
    beforeCreate( ){
      if( !getAuthenticationToken( ) ){
        this.$router.push( {name: 'login'} )
      }
    }
  }
</script>

<style>

</style>
