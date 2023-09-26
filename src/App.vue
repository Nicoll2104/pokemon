<template>
  <div class="todo">
    <div class="barra">
      <h1 class="logo">pokeapi</h1>
    <input type="text" placeholder=" 🔍Buscar" id="barra1">
    <button id="boton">Buscar</button>
  </div>
  <div id="filtro">
    <img src="https://cdn-icons-png.flaticon.com/512/6526/6526846.png" alt="">
    </div>
  <div class="original card-grid">
    <div v-for="pokemon in todo" :key="pokemon.id" class="card">
      <img :src="pokemon.img" class="card-img-top" alt="..." data-bs-toggle="modal" :data-bs-target="'#exampleModal' + pokemon.id">
      <div class="card-body">
        <h5 class="card-title">N°{{ pokemon.id }}</h5>
        <h1 class="card-text">{{ pokemon.nombre }}</h1>
        <p class="tipo" v-for="tipo in pokemon.tipos">{{ tipo }}</p>
      </div>
    </div>
  </div>
  <div>
    <div v-for="pokemon in todo" :key="pokemon.id">
      <div class="modal fade" :id="'exampleModal' + pokemon.id" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h1 class="modal-title fs-5" id="exampleModalLabel">{{ pokemon.nombre }}</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <img :src="pokemon.img" alt="" class="img-fluid" />
              <p>ID: {{ pokemon.id }}</p>
              <p>Altura: {{ pokemon.altura }}</p>
              <p>Peso: {{ pokemon.peso }}</p>
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
import { ref } from "vue";

const todo = ref([]);
const hola = ref(true);

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

for(let i=1; i<=50;i++){
  obtenerUrlsPokemon(i)
}
</script>

<style scoped>

  body {
    font-family: Arial, sans-serif;
    background-color: #f8f8f8;
  }

  h1 {
    text-align: center;
    margin: 20px 0;
    color: #333;
  }

  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(18rem, 1fr));
    gap: 1rem;
    padding: 20px;
  }

  .img-fluid{
    width: 270px;
    height: 250px;
  }
  .card {
    width: 100%;
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  .modal-content {
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  }
  .modal button.btn-close {
    color: #333;
  }
  .stat-container {
    display: flex;
    justify-content: space-between;
    margin-bottom: 8px;
  }

  .stat-name {
    font-weight: bold;
  }

  .stat-progress {
    flex: 1;
    margin-left: 10px;
  }

  .barra {
    color: #fff;
    padding: 10px;
    display: flex;
  }

  .logo {
    font-size: 24px;
    margin: 0;
    padding: 0;
    margin-right: 20px;
  }

  #barra1 {
    flex: 1;
    padding: 1px 1px;
    border: none;
    color: black;
    border: 1px solid black;
    border-radius: 400px;
    width: 8px;
  }

  #boton {
    background-color: #fff;
    color: #007bff;
    border: 1px solid black;
    padding: 8px 16px;
    cursor: pointer;
  }

  #filtro {
    margin-top: 20px;
    margin-left: 40px;
  }

  #filtro img {
    width: 32px;
    height: 32px;
  }
</style>

