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
    <div class="row mx-auto mt-2" v-if="isTeacher">
      <router-link :to="{path:'/crear-curso'}"><button>Crear Curso</button></router-link>
    </div>
    <div class="row">
      <router-view></router-view>
    </div>

  </div>
</template>

<script>
import {getAuthenticationToken, setAuthenticationToken} from '@/dataStorage';
import axios from "axios";
const rolePath = '/mis-roles';
const path = '/profesor/crear-curso';


  export default {
    name: "Home",
    data() {
      return {
        courseName: '',
        durationHours: '',
        roles: [{id: 1, roleName: 'My Role'}],
        isTeacher: false
      }
    },
    beforeCreate() {
      if (!getAuthenticationToken()) {
        this.$router.push({name: 'login'})
      }
    },
    methods: {
      getRole(){
        axios.get(this.$store.state.backURL + rolePath, {params: {access_token: getAuthenticationToken()}})
            .then(response => {
              if (response.status !== 200) {
                alert('Error Obteniendo sus roles');
              } else {
                this.roles = response.data;
              }
            }).catch(error => {
          alert('Error en la petición');
          console.log(error);
        });
      },
      isTeacherRole() {
        if (this.roles[0].roleName === 'Profesor') {
          this.isTeacher = true
        }
      }
    },
    async created() {
      this.getRole();
      await new Promise(r => setTimeout(r, 100));
      this.isTeacherRole();
      console.log(this.roles[0].roleName)
      console.log(this.isTeacher)
    }
  }
</script>

<style>

</style>
