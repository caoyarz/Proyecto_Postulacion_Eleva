<template>
  <div>
    <h1>Hamburguesas</h1>
    <table >
     <tr>
       <th>Nombre</th>
       <th>Calorías</th>
       <th>Ver detalle</th>
     </tr>
     <tr v-for="hamburguesa in hamburguesas" :key="hamburguesa.id">
       <td>{{hamburguesa.nombre}}</td>
       <td>{{hamburguesa.calorias}}</td>
       <td> <img alt="Icono hamburguesa" src="../assets/hamburguesa.png"></td>

     </tr>
    </table>
    
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      hamburguesas: null,
    };
  },
  mounted() {
    this.getHamburguesas();
  },
  methods: {
    getHamburguesas() {
      axios
        .get("https://prueba-hamburguesas.herokuapp.com/burguer")
        .then((response) => {
          this.hamburguesas = response.data;
        })
        .catch((e) => console.log(e));
    },
  },
};
</script>

<style>
table {
  border-collapse: collapse;
  font-size: 12pt;
  font-family: Arial, Helvetica, sans-serif;
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
</style>
