<template>
  <div class="row mx-auto mt-2">
    <form class=" rounded form-inline" @submit="sendForm">

      <div class="form-group col-12">
        <input id="courseName" class="form-control col-12 col-sm-10 col-md-7 offset-sm-1" type="text"
               placeholder="Nombre de Curso" v-model="courseName" required/>
        <input id="durationHours" class="form-control col-12 col-sm-10 col-md-7 offset-sm-1" type="text"
               placeholder="Duracion del Curso" v-model="durationHours" required/>
      </div>

      <div class="col-12 col-sm-5 col-md-4 mb-3">
        <button class="col-sm-10 col-md-10 offset-sm-3 offset-md-4 btn btn-primary" type="submit">
          Crear Formulario
        </button>
      </div>

    </form>
  </div>
</template>

<script>
import axios from "axios";
import {getAuthenticationToken, setAuthenticationToken} from "@/dataStorage";
const path = '/profesor/crear-curso';
export default {


  name: "CourseForm",
  data() {
    return {
      courseName: '',
      durationHours: '',
    }
  },

  methods: {
    sendForm(event) {
      axios
          .post(this.$store.state.backURL + path, // URL
              {
                courseName: this.courseName,
                durationHours: this.durationHours
              },
              {
                headers: {
                  'Content-Type': 'application/json'
                },
                params: {
                  access_token: getAuthenticationToken()
                },
                auth: {
                  username: "soft-eng-ii",
                  password: "secret",
                }
              }
          ).then(response => {
        if (response.status === 201 || respone.status === 200) {
          alert("Curso creado");
          this.courseName = '';
          this.durationHours = ''
          console.log(getAuthenticationToken())
        }
        if (response.status !== 201) {
          alert("Error en la creacion");
          alert(response.status)
        } else {
          setAuthenticationToken(response.data.access_token);
          this.$router.push({name: 'home'});
        }
      }).catch(error => {
        if (error.response.status === 400) {
          alert("Credenciales incorrectas");
        } else {
          alert("¡Parece que hubo un error de comunicación con el servidor!");
        }
      });

      event.preventDefault();
    },
  },
}
</script>

<style scoped>

</style>