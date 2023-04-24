<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

axios.defaults.baseURL = 'https://8080-leo0256-springtopicos20-wjj15pltmnx.ws-us90.gitpod.io';


const nome = ref("Jubileu");
const usuario = ref({
  nome: '', senha: ''
});
const num = ref(0);

const list = ref()

const erro = ref('deu ruin')

function a() {
  num.value++;
}

async function get() {
  try {
    await axios.get('usuario')
    .then(a => {
      list.value = a.data;
      erro.value = ''
    })
    
  } catch (error) {
    erro.value = (error as Error).message;
  }
  /* await axios.get('https://8080-leo0256-springtopicos20-wjj15pltmnx.ws-us90.gitpod.io/usuario').then(({data}) => {
    //list.value = data
    //console.log(data)
  }) */
}

onMounted(() => {
  get();
})

async function cadastrar(e: Event) {
  e.preventDefault()
  await axios.post('/usuario', usuario.value)
  .then(() => get())
}

</script>

<template>
  <div class="about">
    <h1>Welcome, {{ nome }}</h1>
    <p><input type="text" v-model="nome"/></p>
    <p v-if="nome.length > 10">Um mega super master nome incrivelmente longo</p>
    <p v-else>Um nome ai</p>

    <p>{{ num }} <button @click="a()">Incrementar</button></p>

    <!-- <button @click="get()">Click me</button> -->
    <p v-if="erro">{{ erro }}</p>

    <form @submit="e => cadastrar(e)">
      <p><input type="text" v-model="usuario.nome"/></p>
      <p><input type="text" v-model="usuario.senha"/></p>
      <button type="submit">cadastrar</button>
    </form>
    

    <table>
      <thead>
        <td>Id</td>
        <td>Nome</td>
        <td>Valor</td>
      </thead>
      <tbody>
        <tr v-for="item in list" :key="item.id">
          <td>{{ item.id }}</td>
          <td>{{ item.nome }}</td>
          <td>{{ item.senha }}</td>
        </tr>
      </tbody>
    </table>
  </div>
  
  
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
}
</style>
