<template>
  <div class="p-3 row">  <!--Crea un contenedor con un padding de 3  y una clase de fila para alinear los elementos horizontalmente -->
    <div class="cardPaciente col-12 col-md-4 col-lg-4 p-3" :style="colorCard()"> <!--La función colorCard se está utilizando para aplicar estilos dinámicos a la tarjeta.-->
      <div class="cardTitle text-center"> <!--Este elemento contiene el titulo con la información del paciente-->
        <h3>Paciente</h3>
        <p>{{ paciente }}</p>
      </div>
      <div class="cardBody text-center"> <!--Este elemento contiene el cuerpo de la tarjeta, con la información de la fecha, la hora  y el motivo de la consulta-->
        <h3>Fecha</h3>
        {{ fecha }}
        <h3>Hora</h3>
        {{ hora }}
        <h3>Motivo</h3>
        {{ motivo }}
      </div>
      <div class="cardFooter text-center p-2">  <!--Este elemento cardFooter, contiene un botón con la clase btn btn-info que al hacer click emitirá un evento llamado eliminarConsulta-->
        <button class="btn btn-info" @click="$emit('eliminarConsulta')">
          Eliminar
        </button>
      </div>
    </div>
  </div>
</template><!--En resumen , este código representa una tarjeta de información de una consulta médica en un formato visualmente atractivo y estructurado, con la capacidad de eliminar la consulta al hacer click en un botón-->

<script>
export default {
  name: "CardPaciente",   /*Este código es un componente de Vue llamado CardPaciente, el cual recibe propiedades(props) paciente, fecha,hora, gravedad y motivo. El propósito de este componente es mostrar información de un paciente en forma de tarjeta, y además cambiar el color de la tarjeta dependiendo del nivel de gravedad del paciente */
  props: {
    paciente: String,
    fecha: String,
    hora: String,
    gravedad: String,
    motivo: String,
  },
  methods: {    /*El método colorCard se encarga de definir el color de fondo y el color de texto de la tarjeta según el nivel de gravedad del paciente */
    colorCard: function () {  /*Si la gravedad es baja el fondo será verde y el texto negro, si la gravedad es media el fondo será amarillo y el texto negro, sila gravedad es alta, el fondo será rojo y el texto blanco */
      if (this.gravedad === "baja") {
        return { backgroundColor: "green", color: "black" };
      } else if (this.gravedad === "media") {
        return { backgroundColor: "yellow", color: "black" };
      } else if (this.gravedad === "alta") {
        return { backgroundColor: "red", color: "white" };
      }
      console.log(this.gravedad); /*Muestra en la consola el valor de this.gravedad */
    },
  },
};
</script><!--En resumen, este componente de Vue representa una tarjeta para mostrar información de un paciente, cambiando el color de la tarjeta dependiendo del nivel de gravedad del paciente.-->

<style scoped>  /*El estilo que se está aplicando al componente es que tenga un ancho de 300px. Al utilizar la propiedad de scoped, se limita la aplicación de este estiloúnicamente al componente en el cual está definido, evitando que se aplique a otros componentes de la aplicación  */
.cardPaciente{
    width: 300px;
}
</style>
