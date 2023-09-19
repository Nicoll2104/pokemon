<script setup>
import axios from "axios";
import { ref } from "vue";

const todo = ref("");
const hola = ref(true);

async function obtenerUrlsPokemon() {
  let r = await axios.get("https://pokeapi.co/api/v2/pokemon/25/");
  console.log(r);
  todo.value = {
    id: r.data.id,
    img: r.data.sprites.other["official-artwork"].front_default,
    nombre: r.data.name,
    altura: r.data.height,
    peso: r.data.weight,
    estadisticas: r.data.stats.map((e) => {
      return { name: e.stat.name, cant: e.base_stat };
    }),
    tipos: r.data.types.map((e) => e.type.name),
  };
}

const concatenar = (cant) =>{
  let a = 'width: '
  a += cant
  a += "%"
  return a
}
</script>

<template>
  <div class="original">
    <div v-if="hola">
      <h1 class="id">#{{ todo.id }}</h1>
      <h1 class="nombre">{{ todo.nombre }}</h1>
      <div v-for="(tipo, index) in todo.tipos" :key="index">
        {{ tipo }}
      </div>
      <div class="juntos">
        <h1>Altura:</h1>
        <p>{{ todo.altura }}</p>
        <h1>peso:</h1>
        <p>{{ todo.peso }} KG</p>
      </div>
      <div class="esta">
        <h1>estadisticas</h1>
        <div class="esta1">
          <div v-for="(stat, index) in todo.estadisticas" :key="index">
            <p>{{ stat.name }}</p>
            <div style="width: 100%; height: 25px; background-color: rgb(209, 169, 169); border-radius:50px ;">
              <div :style="concatenar(stat.cant)" class="barra" >{{ stat.cant }} </div>
            </div>
            <p>{{ stat.cant }}</p>
          </div>
        </div>
      </div>
      <div class="body">
        <button @click="obtenerUrlsPokemon()" class="boton">peticion</button>
        <img :src="todo.img" alt="" />
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

.esta1{
  width: 500px;
}
</style>
