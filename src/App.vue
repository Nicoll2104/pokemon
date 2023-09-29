<template>
  <div class="todo">
    <div class="arriba">
      <div class="logo">
      <img src="/src/assets/img.png" alt="" id="logo">
    </div>
    <div class="barra">
    <input type="text" placeholder=" 🔍Buscar" id="barra1" v-model="txtBuscar">
    <button id="boton" @click="buscar()">Buscar</button>
  </div>
</div>
  <div id="filtro">
    <p class="d-inline-flex gap-1">
  <button  id="filtro1" class="btn btn-primary" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
    <img src="https://cdn-icons-png.flaticon.com/512/6526/6526846.png" alt="">
    <h2>Filtrar</h2>
  </button>
</p>
<div class="collapse" id="collapseExample" >
  <div class="card card-body" id="opciones">
    <div v-for="(opcion, index) in opciones" :key="index" id="opcion1">
      <input type="radio" :id="'opcion' + (index + 1)" :name="'grupoOpciones'" :value="opcion" v-model="opcionSeleccionada">
      <label :for="'opcion' + (index + 1)">{{ opcion }}</label><br>
    </div>
  </div>
</div>
  </div>
    <div class="card-grid" v-if="componenteBuscar">
      <div class="card">
      <img :src="buscado.img" class="card-img-top" alt="..." data-bs-toggle="modal" :data-bs-target="'#exampleModal' + buscado.id">
      <div class="card-body">
        <h5 class="card-title">N°{{ buscado.id }}</h5>
        <h1 class="card-text">{{ buscado.nombre }}</h1>
        <p class="tipo" v-for="tipo in buscado.tipos" :style="'background-color:' + colores[tipo]">{{ tipo }}</p>
      </div>
    </div>
  </div>
  <div class=" card-grid" v-if="!componenteBuscar">
    <div v-for="pokemon in todo" :key="pokemon.id" class="card">
      <img :src="pokemon.img" class="card-img-top" alt="..." data-bs-toggle="modal" :data-bs-target="'#exampleModal' + pokemon.id">
      <div class="card-body">
        <h5 class="card-title">N°{{ pokemon.id }}</h5>
        <h1 class="card-text">{{ pokemon.nombre }}</h1>
        <p class="tipo" v-for="tipo in pokemon.tipos" :style="'background-color:' + colores[tipo]">{{ tipo }}</p>
      </div>
    </div>
    <div class="ultimo">
    <button @click="mostrar()" id="ultimo"> Ver más</button>
  </div>
  </div>
  <div>
    <div v-for="pokemon in todo" :key="pokemon.id">
      <div class="modal fade" :id="'exampleModal' + pokemon.id" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <h1 class="id-modal">#{{ pokemon.id }}</h1>
              <h1 class="nombre-modal">{{ pokemon.nombre }}</h1>
              <img :src="pokemon.img" alt="" class="img-fluid" />
              <p class="tipo" id="tipo-modal" v-for="tipo in pokemon.tipos" :style="'background-color:' + colores[tipo]">{{ tipo }}</p>
              <p class="igual-modal"><span class="bold-text">Altura:</span> {{ pokemon.altura }}</p>
              <p class="igual-modal"><span class="bold-text">Peso:</span> {{ pokemon.peso }}</p>
              <h2>Estadísticas:</h2>
              <div v-for="stat in pokemon.estadisticas" :key="stat.name">
                <div class="stat-container">
                  <div class="stat-name">{{ stat.name }}</div>
                  <div class="stat-progress">
                    <div class="progress" role="progressbar" aria-valuenow="0" :aria-valuenow="stat.cant" aria-valuemin="0" aria-valuemax="100">
                      <div class="progress-bar progress-bar-striped progress-bar-animated" :style="{ width: stat.cant + '%' }"></div>
                    </div>
                  </div>
                  <div class="stat-amount">{{ stat.cant }}</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script setup>
import axios from "axios";
import { ref, computed } from "vue";

const todo = ref([]);
const colores = {
  grass: "#689F38 ",
  poison: "#b50d82",
  fire: "#ff7404",
  flying: "#A890F0",
  water: "#3498DB",
  bug: "#A8b820",
  normal: "#fdddca",
  electric:"#cfa055",
  ground: "#795548",
  fairy: "#FF69B4",
  fighting: "#CC0000",
  psychic: "#FFFF00",
  rock: "#b8b6ad",
  steel: "#a6cad4",
  ice:"#e7eff6",
  ghost:"#8f728e"
};
const componenteBuscar = ref(false); 
const opciones = ['grass','poison','fire','flying','water','bug','normal','electric','ground','fairy','fighting','psychic','rock','steel','ice','ghost'];

let cant = 1
let limites = 50

function mostrar(){
  for(cant; cant <= limites; cant ++){
    obtenerUrlsPokemon(cant)
  }
  limites += 50
  console.log(todo.value);
}

mostrar()

async function obtenerUrlsPokemon(i) {
  let r = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}/`);
  console.log(r);
  todo.value.push({
    id: r.data.id,
    img: r.data.sprites.other["official-artwork"].front_default,
    nombre: r.data.name,
    altura: r.data.height,
    peso: r.data.weight,
    estadisticas: r.data.stats.map((e) => {
      return { name: e.stat.name, cant: e.base_stat };
    }),
    tipos: r.data.types.map((e) => e.type.name),
  });
}

const txtBuscar = ref("")
const buscado = ref({})

function buscar(){
  buscado.value = todo.value.find(s =>s.nombre == txtBuscar.value)
  componenteBuscar.value = true

}

</script>

<style scoped>

  body {
    font-family: Arial, sans-serif;
    background-color: #f8f8f8;
  }

  #logo{
    width: 120px;
    height: 50px;
  }

  h1 {
    text-align: center;
    margin: 20px 0;
    color: #333;
  }

  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(20rem, 1fr));
    gap: 1rem;
    padding: 20px;
  }

  .img-fluid{
    width: 570px;
    height: 550px;
    margin-left: 30%;
  }

  .card-img-top{
    background-color: rgb(229, 229, 229);
  } 
  .card {
    width: 100%;
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .card-text{
    font-size: 25px;
  }
  .modal-content {
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  }

  .nombre-modal{
    font-size: 55px;
    margin-top: 1%;
    
  }

  .id-modal{
    font-size: 55px;
    margin-right: 90%;
    margin-top: 1%;
    margin-bottom: 1%;
  }

  .igual-modal{
    font-size: 25px;
  }

  .bold-text {
    font-weight: bold;
    font-size: 25px;
  }

  .modal-dialog{
    max-width: 80%;
    width: 80%;
  }

  #tipo-modal{
    font-size: 120%;
    
  }
  .modal button.btn-close {
    color: #333;
  }
  .stat-container {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
  }

  .stat-name {
    font-size: 18px;
    font-weight: bold;
  }

  .stat-progress {
    flex: 1;
    margin-left: 10px;
    height: 30px;
    margin-bottom: 5px;
  }

  .progress-bar {
  height: 900%; 
  font-size: 16px;
  }
  
  .arriba{
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 20px;
  }

  .barra {
    color: #fff;
    height: 60px;
  }

  .logo {
  margin-right: 20px;
  }

  #barra1 {
    flex: 1;
    padding: 1px 1px;
    border: none;
    color: black;
    border: 1px solid black;
    border-radius: 400px;
    width: 450px;
    height: 38px;
  }

  #boton {
    background-color: #007bff;
    border: 1px solid black;
    padding: 8px 16px;
    margin-left:30px ;
    border-radius: 400px;
    font-weight: bold;
  }

  #filtro {
    margin-top: 20px;
    margin-left: 40px;
  }

  #filtro img {
    width: 32px;
    height: 32px;
  }

  .tipo{
    width: 70px;
    border-radius: 400px;
    text-align: center;
    display: flex;
    flex-direction:column;
  }

 #filtro1{
  display: flex;
  flex-direction: row;
  background-color: transparent;
  border: 1px solid transparent;
  color: black;
 }


 .card-body {
    background-color: #f7f7f7;
    padding: 20px;
    border-radius: 10px;
  }

  #opciones{
    display: flex;
    flex-direction: row;
    margin-right: 600%;
  }

  #opcion1{
    text-align: center;
    justify-content: center;
    padding: 15px;
  }

  #collapseExample{
    margin-right: 40px;
  }

  #ultimo{
    height: 40px;
    width: 90px;
    font-weight: bold;
    background-color: #9bfae0;
  }

</style>

