<template>
  <div class="container">
    <form>
      <div class="form-group">
        <div class="row">
          <div class="col-md">
            <label for="exampleFormControlInput1">Nombre</label>
          </div>
          <div class="col-md">
            <input
              v-model="nombre"
              type="text"
              class="form-control"
              id="exampleFormControlInput1"
              placeholder="nombre"
            />
          </div>
        </div>
        <p></p>
        <div class="form-group">
          <div class="row">
            <div class="col-md text-center">
              <label for="exampleFormControlSelect1">Raza</label>
            </div>
            <div class="col-md text-center">
              <select class="form-control" id="raza" v-model="raza">
                <option disabled value>Seleccione un elemento</option>
                <option>C</option>
                <option>A</option>
                <option>S</option>
              </select>
            </div>
          </div>
        </div>
      </div>
    </form>

    <p></p>

    <button type="button" class="btn btn-success" @click="insertToga">Insertar</button>

    <button type="button" class="btn btn-primary ml-1" @click="getTogas">Cargar</button>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">id</th>
          <th scope="col">Nombre</th>
          <th scope="col">Raza</th>
          <th scope="col">Sexo</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in gatolates" :key="item.key">
          <td>{{index}}</td>
          <td>{{item.id}}</td>
          <td>{{item.nombre}}</td>
          <td>{{item.raza}}</td>
          <td>{{item.sexo}}</td>
          <td>
            <button class="btn btn-warning mr-1">editar</button>
            <button class="btn btn-danger" @click="deleteToga(item)">eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
import axios from "axios";
import "isomorphic-fetch";

export default {
  name: "Gato",
  data() {
    return {
      gatolates: [],

      nombre: "",
      raza: "",
      sexo: "F",
      id: ""
    };
  },
  mounted(){
    this.getTogas()
  },
  methods: {
    getTogas() {
      axios
        .get("http://127.0.0.1:8000/rest/gatos/")
        .then(response => (this.gatolates = response.data))
        .catch(error => console.log(error));
    },
    insertToga() {
      const path = "http://127.0.0.1:8000/rest/gatos/";

      axios
        .post(path, {
          nombre: this.nombre,
          raza: this.raza,
          sexo: this.sexo
        })
        .then(response => {
          console.log(response.data);
          (this.nombre = ""), (this.raza = "");
          this.getTogas();
        })
        .catch(error => console.log(error));
    },
    deleteToga(item) {
      axios
        .delete("http://127.0.0.1:8000/rest/gatos/" + item.id)
        .then(response => {
          console.log(response.data);
          this.getTogas();
        });
    }
  }
};
</script>