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

<template>
  <div class="original">
    <div v-for="pokemon in todo" :key="pokemon.id" class="card" style="width: 18rem;">
      <img :src="pokemon.img" class="card-img-top" alt="...">
      <div class="card-body">
        <h5 class="card-title">{{ pokemon.id }}</h5>
        <p class="card-text">{{ pokemon.nombre }}</p>
        <h2 class="tipo" v-for="tipo in pokemon.tipos">{{ tipo }}</h2>
      </div>
    </div>
  </div>
</template>


<style scoped>
.id {
  color: #73c6b6;
  font-size: 150px;
  margin: 0%;
}

.nombre {
  font-size: 60px;
  font-weight: bold;
  margin: 0 0 1px 240px;
}

.body {
  display: flex;
}

.barra{
  height: 25px;
  border-radius: 50px;
  background-color: red;
}

.esta{
  display: flex;
  flex-direction: row;
}

.esta1{
  width: 500px;
  margin-left: 60px;
}

.abajo{
  font-size: 45px;
  margin: 200px 0 0 50px;
  text-align: center;
  justify-content: center;
}


.juntos{
  display: flex;
  flex-direction: row;
  line-height: 1%;
}

.juntos1{
  margin-left: 50px;
}
</style>