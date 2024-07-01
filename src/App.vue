<template>
  <div id="app" class="mt-3"><!--Se define el contenedor principal con ei id App y se le asigna la clase mt-3-->
    <main class="container p-3">
      <div class="text-center p-3"><!--Se define título principal "Cunsulta Médica" dentro de un div con la clase text.center-->
        <h1>Consulta Médica</h1>
      </div>
      <form @submit.prevent="addConsulta"><!--Se crea un formulario con la directiva @submit.prevent=addConsulta, que llama a un método addConsulta cuando el formulario se envía-->
        <div class="row">
          <div class="col-12 col-md-6 col-lg-2">
            <div class="text-center"><!--Dentro del formulario se definen campos de ingresar datosdel paciente, como: nombre, fecha, hora, gravedad y motivo de la consulta. Estos campos están vinculados a propiedades del objeto nuevasConsulta a través de la directiva v-model-->
              <label for="paciente" v-if="nuevasConsulta.paciente" style="color: black">Paciente</label>
              <label for="paciente" v-else style="color: red">Paciente</label><br />
              <input type="text" class="inputPaciente" name="paciente" required v-model="nuevasConsulta.paciente" />
            </div>
          </div>

          <div class="col-12 col-md-6 col-lg-2">
            <div class="text-center">
              <label for="fecha" v-if="nuevasConsulta.fecha" style="color: black">Fecha</label>
              <label for="fecha" v-else style="color: red">Fecha</label><br />
              <input type="date" class="inputFecha" name="fecha" required v-model="nuevasConsulta.fecha" />
            </div>
          </div>

          <div class="col-12 col-md-6 col-lg-2 inputs">
            <div class="text-center">
              <label for="hora" v-if="nuevasConsulta.hora" style="color: black">Hora</label>
              <label for="hora" v-else style="color: red">Fecha</label><br />
              <input type="time" class="inputHora" name="hora" required v-model="nuevasConsulta.hora" />
            </div>
          </div>

          <div class="col-12 col-md-6 col-lg-2">
            <div class="text-center">
              <label for="gravedad" v-if="nuevasConsulta.gravedad" style="color: black">Gravedad</label>
              <label for="gravedad" v-else style="color: red">Gravedad</label><br />
              <select name="gravedad" class="inputGravedad" v-model="nuevasConsulta.gravedad">
                <option disabled value="null" >Selecciona gravedad</option>
                <option v-for="(tipoGravedad, index) in tiposGravedad" :key="index" :value="tipoGravedad">
                  {{ tipoGravedad }}
                </option>
              </select>
            </div>
          </div>

          <div class="col-12 col-md-6 col-lg-2">
            <div class="text-center">
              <label for="motivo" v-if="nuevasConsulta.motivo" style="color: black">Motivo</label>
              <label for="motivo" v-else style="color: red">Motivo</label><br />
              <textarea required class="inputMotivo" v-model="nuevasConsulta.motivo"></textarea>
            </div>
          </div>
          <div class="text-center">
            <button type="submit" class="btn btn-info" :disabled="!botonActivo">Agregar</button><!--Se incluye botón de agregar que estará activo (no estará desabilitado) cuando la propiedad botonActivo sea verdadera-->
          </div>
        </div>
      </form>
    </main>
    <div class="text-center">
      <p v-show="consultas.length === 0" style="color: red">Aún no hay consultas registradas</p><!--Se muestra un mensaje en color rojo si no hay consultas registradasen la lista de consultas-->
    </div>
    <hr>
    <section class="sectionCard p-5"><!--Se incluye un componente llamado CardPaciente que se mostrará para cada elemento en la lista de consultas. Este componente recibe las propiedades del paciente, fecha, hora, gravedad y motivo de la consulta , y también tiene un evento para eliminar una consulta específica -->
      <CardPaciente v-for="(consulta, index) in consultas" :key="index" :paciente="consulta.paciente"   
        :fecha="consulta.fecha" :hora="consulta.hora" :gravedad="consulta.gravedad" :motivo="consulta.motivo"
        @eliminarConsulta="eliminarConsulta(index)" />
    </section>

  </div><!--Este código define la interfaz de usuario de una aplicación de consulta médica, con campos para agregar nuevas consultas, mostrar un mensaje si no hay consultas registradas y mostrar una lista de consultas de consultas existentes en forma de tarjetas-->
</template>

<script>
import CardPaciente from "./components/CardPaciente.vue";      /*Se importa el componente CardPaciente desde el archivo CardPaciente.vue */
export default {
  name: "App",  /*se define el componente principal con el nombre App */
  components: {   /*Se especifica que el componente CardPaciente es parte del componente principal */
    CardPaciente
  },
  data() {    /*Se define un objeto dataque contiene 2 arrays: tiposGravedad y consultas; y un objeto nuevasConsulta con las propiedades de paciente, fecha hora gravedad y motivo */
    return {
      tiposGravedad: ["baja", "media", "alta"],
      consultas: [],
      nuevasConsulta: {
        paciente: "",
        fecha: "",
        hora: "",
        gravedad: null,
        motivo: "",
      },
    };
  },
  methods: {    /*Se definen 2 métodos: addConsulta que añade la nueva consulta al array consultas y limpia el formulario, y eliminarConsulta que elimina una consulta específica del array consultas */
    addConsulta: function () {
      this.consultas.push(this.nuevasConsulta);
      console.log(this.consultas);
      //Limpiar formulario
      this.nuevasConsulta = {
        paciente: "",
        fecha: "",
        hora: "",
        gravedad: "",
        motivo: "",
      };
    },
    eliminarConsulta: function (index) {
      let consulta = this.consultas[index];
      if (confirm(`Esta seguro que desea eliminar la cita de ${consulta.paciente}?`)) {
        this.consultas.splice(index, 1);
      }
    }
  },
  computed: {   /*Se define un computed property llamado botonActivo que devuelve true si todas las propiedades del objeto nuevasConsulta tienen un valor */
    botonActivo: function () {
      return (
        this.nuevasConsulta.paciente &&
        this.nuevasConsulta.fecha &&
        this.nuevasConsulta.hora &&
        this.nuevasConsulta.gravedad &&
        this.nuevasConsulta.motivo
      );
    },
  },
};
</script><!--En resumen este componente Vue permite añadir y eliminar consultas médicas y valida que todos los campos del formulario estén completos antes de añadir una nueva consulta-->

<style>
main {
  border: 1px solid black;
  background-color: rgb(243, 206, 245); /*Color de fondo de main */
}
.row{
  display: flex;
  justify-content: space-around;
}
.inputPaciente{
  width: 165px;
}
.inputFecha{
  width: 165px;
}
.inputHora{
  width: 165px;
}
.inputGravedad{
  width: 165px;
}
.inputMotivo{
  width: 165px;
}

.sectionCard{
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap
}
</style>
