<template>
  <div>
     <center><img src="../assets/hamburguesa3.png" alt=""></center>
    <div class="titulo">
    Hamburguesas
     
    </div>
   <!-----------------------------TABLA PARA MOSTRAR EL CRUD------------------- -->
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
        <td><button class="boton_modifica" @click="actualizaHamburguesas(hamburguesa._id)"  ></button> 
        <button class="boton_elimina"  @click="confirmarEliminacion(hamburguesa._id)"></button></td>
     </tr>
    </table>    

    <!--  POPUP PARA MOSTRAR LOS DETALLES-------------------------------------->
    <div class="overlay"  v-if="detalles">
      <div  class="ventana_detalles">       
        <strong>Nombre:</strong> {{hamburguesaSeleccionada[0].nombre}}<br>
        <strong>Calorías:</strong> {{hamburguesaSeleccionada[0].calorias}}<br>
        <strong>Ingredientes:</strong>
        <ul class="ingrediente">
        <li  v-for="ingrediente in hamburguesaSeleccionada[0].ingredientes" :key="ingrediente">{{ingrediente}}</li>
        </ul>       
        <button @click="detalles=false" class="boton_cerrar_info">Cerrar</button>
      </div>
    </div>

    
    <!--POPUP PARA MODIFICAR UNA HAMBURGUESA------------------------------------->
    <div class="overlay"  v-if="modificar">
      <div  class="ventana_detalles">       
        <strong>Nombre: </strong> 
        <input type="text" v-bind:placeholder="hamburguesaSeleccionada[0].nombre" v-model="nombreM"><br>
        <strong>Calorías:</strong> 
        <input type="number" v-bind:placeholder="hamburguesaSeleccionada[0].calorias" v-model="caloriasM"><br>
        <strong>Ingredientes:</strong>
        <ul class="ingrediente">
        <li  v-for="ingrediente in hamburguesaSeleccionada[0].ingredientes" :key="ingrediente"><input type="text" v-bind:placeholder="ingrediente" v-model="ingredientesM"></li>
        </ul>       
        <button @click="actualizarHamburguesa(hamburguesaSeleccionada._id,)" class="boton_actualizar">Actualizar</button>
        <br>
        <button @click="modificar=false" class="boton_cerrar_info">Cerrar</button>
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
      detalles: false,
      modificar: false,
      hamburguesaSeleccionada: {},
      //variables que guardan los datos de la modificaion
      nombreM: "",
      caloriasM: "",
      ingredientesM: [],
    };
  },
  mounted() {
    this.getHamburguesas();
  },
  methods: {
    //CONEXION A LA API PARA TRAER LOS DATOS
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
      this.detalles = true;
    },
    //FUNCION QUE CONFIRMA LA ELIMINACION MEDIANTE UNA VENTANA DE ALERTA
    confirmarEliminacion: function(id) {
      this.hamburguesaSeleccionada = this.hamburguesas.filter(
        (burger) => burger._id === id
      );
      var respuesta = confirm("Confirmar la eliminación del registro");

      if (respuesta) {
        this.eliminarHamburguesa(id);
      }
    },
    //FUNCION QUE ELIMINA
    eliminarHamburguesa: function(id) {
      axios
        .delete("https://prueba-hamburguesas.herokuapp.com/burguer/" + id)
        .then((respuesta) => {
          this.getHamburguesas();
        })
        .catch((e) => console.log(e));
    },
    //SELECCIONA UNA HAMBURGUESA Y LLAMA A LA VENTANA DONDE SE MUESTRAN LOS DATOS PARA ACTUALIZAR
    actualizaHamburguesas: function(id) {
      this.hamburguesaSeleccionada = this.hamburguesas.filter(
        (burger) => burger._id === id
      );
      this.modificar = true;
    },

    // PUT PARA MODIFICAR LA INFORMACION -- NO LOGRADO
    actualizarHamburguesa: function(id) {
      axios
        .put("https://prueba-hamburguesas.herokuapp.com/burguer/" + id, {
          nombre: this.nombreM,
          calorias: this.caloriasM,
          ingredientes: this.ingredientesM,
        })
        .then((respuesta) => {
          this.getHamburguesas();
        })
        .catch((e) => console.log(e));
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
  background-image: url("../assets/hamburguesa2.png");
  background-size: cover;
  border: none;
  padding: 18px;
  cursor: pointer;
  transition: all 0.5s;
  background-color: transparent;
}
.boton_detalles:hover {
  width: 45px;
  height: 45px;
}
.boton_elimina {
  background-image: url("../assets/eliminar.png");
  background-size: cover;
  border: none;
  padding: 18px;
  cursor: pointer;
  transition: all 0.5s;
  background-color: transparent;
}
.boton_elimina:hover {
  width: 45px;
  height: 45px;
}
.boton_modifica {
  background-image: url("../assets/editar.png");
  background-size: cover;
  border: none;
  padding: 18px;
  cursor: pointer;
  transition: all 0.5s;
  background-color: transparent;
}
.boton_modifica:hover {
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
  background: white;
  box-shadow: 0px 0px 5px 5px rgb(0, 0, 0, 0.3);
  border-radius: 10px;
  padding: 30px;
  width: 350px;
  height: max-content;
  text-align: left;
}

.ingrediente {
  list-style: none;
  display: inline-flex;
  flex-direction: column;
  align-items: left;
}
.titulo {
  display: inline-flex;
  font-weight: bold;
  font-size: 20pt;
  flex-direction: column;
}
.ventana_agregar {
  background: white;
  box-shadow: 0px 0px 5px 5px rgb(0, 0, 0, 0.3);
  border-radius: 10px;
  padding: 30px;
  width: 350px;
  height: max-content;
  text-align: left;
  display: inline-flex;
  flex-direction: column;
  align-items: left;
}
.ventana_agregar label {
  font-weight: bold;
  padding: 5px;
}
input {
  border: 0;
}
.boton_actualizar {
  background: #3551ad;
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
.boton_actualizar:hover {
  background: #273c80;
}
</style>
