<template>
  <div>
    <h1>Hamburguesas</h1>
    <button class="boton_nueva">Nueva Hamburguesa</button>
    <table >
     <tr>
       <th>Nombre</th>
       <th>Calorías</th>
       <th>Ver detalle</th>
       <th>Acciones</th>
     </tr>
     <tr v-for="hamburguesa in hamburguesas" :key="hamburguesa.id">
       <td>{{hamburguesa.nombre}}</td>
       <td>{{hamburguesa.calorias}}</td>       
       <td> <button @click="detalleHamburguesas(hamburguesa._id)"  class="boton_detalles" ></button></td>
       <!--<td> <button @click.prevent="ventana=true" class="boton_detalles" :key="hamburguesa.id" >Modificar</button></td>
       <td> <button @click.prevent="ventana=true" class="boton_detalles" :key="hamburguesa.id" >Eliminar</button></td>-->
     </tr>
    </table>    

    <div class="overlay" v-bind="ventana" v-if="ventana">
      <div  class="ventana_detalles">       
        <strong>Nombre:</strong> {{hamburguesaSeleccionada[0].nombre}}<br>
        <strong>Calorías:</strong>{{hamburguesaSeleccionada[0].calorias}}<br>
        <strong>Ingredientes:</strong>
        <ul class="ingrediente">
        <li  v-for="ingrediente in hamburguesaSeleccionada[0].ingredientes" :key="ingrediente">{{ingrediente}}</li>
        </ul>
       
        <button @click.prevent="ventana=false" class="boton_cerrar_info">Cerrar</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      hamburguesas: null,
      ventana: false,
      hamburguesaSeleccionada: null,
    };
  },
  mounted() {
    this.getHamburguesas();
  },
  methods: {
    getHamburguesas() {
      axios
        .get("https://prueba-hamburguesas.herokuapp.com/burguer")
        .then((respuesta) => {
          this.hamburguesas = respuesta.data;
        })
        .catch((e) => console.log(e));
    },

    detalleHamburguesas: function(id) {
      this.hamburguesaSeleccionada = this.hamburguesas.filter(
        (burger) => burger._id === id
      );

      this.ventana = true;
    },
  },
};
</script>

<style>
template {
  font-family: Arial, Helvetica, sans-serif;
}
table {
  border-collapse: collapse;
  font-size: 12pt;
  width: 80%;
  text-align: center;
  margin: auto;
}
th {
  text-align: center;
  padding: 10px;
  background: #42b983;
  color: #fff;
}
td {
  padding: 1px;
}

table tr:nth-child(odd) {
  background: rgba(250, 245, 245, 0.945);
}
.boton_detalles {
  background-image: url("../assets/hamburguesa.png");
  background-size: cover;
  border: none;
  padding: 20px;
  cursor: pointer;
  transition: all 0.5s;
}
.boton_detalles:hover {
  width: 45px;
  height: 45px;
}

.boton_nueva {
  background: #42b983;
  border-radius: 15px;
  padding: 10px;
  margin: 10px;
  border: 0px;
  color: white;
  font-weight: bold;
  font-size: 12pt;
  cursor: pointer;
}
.boton_nueva:hover {
  background: #30855f;
}
.boton_cerrar_info {
  background: #42b983;
  border-radius: 15px;
  padding: 10px;
  margin: 10px;
  border: 0px;
  color: white;
  font-weight: bold;
  font-size: 12pt;
  cursor: pointer;
  display: flex;
  margin: auto;
  justify-content: center;
}
.boton_cerrar_info:hover {
  background: #30855f;
}
.overlay {
  background: rgb(0, 0, 0, 0.3);
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
.ventana_detalles {
  background: #f8f8f8;
  box-shadow: 0px 0px 5px 5px rgb(0, 0, 0, 0.3);
  border-radius: 10px;
  padding: 30px;
  width: 350px;
  height: auto;
  text-align: left;
}

.ingrediente {
  list-style: none;
  display: inline-flex;
  flex-direction: column;
  align-items: left;
}
</style>
