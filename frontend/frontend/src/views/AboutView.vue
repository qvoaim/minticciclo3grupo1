<template>
  <table allign="left">
    <h1>ENTIDAD</h1>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">NOMBRE</th>
      <th scope="col">DIRECCION</th>
    </tr>
    <tr>
      <td>
        <input type="text" id="id" name="id" v-model="id">
      </td>
      <td>
        <input type="text" id="nombre" name="nombre" v-model="nombre">
      </td>
      <td>
        <input type="text" id="direccion" name="direccion" v-model="direccion">
      </td>
    </tr>
    <tr>
      <button v-on:click="registrar()">CREAR</button>
    </tr>
    <br><br><br>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">NOMBRE</th>
      <th scope="col">DIRECCION</th>
    </tr>
    <tr v-for="i in Lista">
      <td>{{ i.id }}</td>
      <td>{{ i.nombre }}</td>
      <td>{{ i.direccion }}</td>
      <td>
        <button v-on:click="eliminar(i.id)">DELETE</button>
      </td>
      <td>
        <button v-on:click="editar(i.id)">UPDATE</button>
      </td>
    </tr>
  </table>
</template>
<script>
  import axios from 'axios';

  export default {
    name: "AboutView",
    data() {
      return {
        Lista: null,
        id: "",
        nombre: "",
        direccion: "",
      }
    },
    methods: {
      registrar() {
        let post = {
          "id": this.id,
          "nombre": this.nombre,
          "direccion": this.direccion,
        }
        axios.post("http://127.0.0.1:8000/api/entidad/", post)
            .then(result => {
              this.id="";
              this.nombre="";
              this.direccion="";
              this.updated()
            })
      },
      editar(id) {
        console.log(id)
        this.$router.push('edit/' + id);
      },
      eliminar(id) {
        console.log(id)
        axios.delete("http://127.0.0.1:8000/api/entidad/" + id +"/").then(result => {
          this.update()
          console.log(result);
        })
      },
      updated() {
        let direccion = "http://127.0.0.1:8000/api/entidad/";
        axios.get(direccion).then(data => {
          this.Lista = data.data;
        })
      },
    },
    mounted() {
      let direccion = "http://127.0.0.1:8000/api/entidad/";
      axios.get(direccion).then(data => {
        this.Lista = data.data;
        console.log(data);         
      })
    },
  }
</script>
  <style>

  </style>
