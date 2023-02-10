<script setup>
  import { ref } from 'vue';

  const name = ref('');
  const message = ref('Digite o nome de um pokémon para saber todos os detalhes dele');
  const pokemon = ref({});

  function searchPokemon(){
    const pokemonName = String(name.value).toLowerCase();

    pokemon.value = {}
    message.value = 'Estamos buscando o seu pokemon'

    fetch(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`)
    .then(response => response.json())
    .then(response => {
      pokemon.value = response
      message.value = 'Digitando o nome de um pokémon você para todos os detalhes dele'
    })
    .catch(error => {
      console.log(error)
      message.value = 'Não encontramos esse pokemon, tente pesquisar novamente!'
    })
  }
</script>

<template>
  <main>
    <form action="" @submit.prevent="searchPokemon">
      <input 
        type="text" 
        id="namePokemon" 
        placeholder="Qual pokémon você está procurando?" 
        v-model="name" 
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
      />
    </form>
  </main>

  <template v-if="Object.keys(pokemon).length !== 0">
    
    <div class="mt-4">
      <h1 class="flex justify-center items-center text-3xl font-semibold">{{ pokemon.name }}</h1>
      <div class="rounded-lg bg-sky-200 w-60 h-60 mt-10 flex justify-between border-2 border-black">
        <img :src="pokemon.sprites.other.home.front_default" class="h-52 w-52 flex justify-between"/>
      </div>
      <div class="mt-24 flex flex-row items-center justify-between px-20 border-2 border-black rounded-lg">
        <h1 class="text-lg"><strong>HP: </strong>123</h1>
        <h1 class="text-lg"><strong>Attack: </strong>123</h1>
        <h1 class="text-lg"><strong>Defense: </strong>123</h1>
        <h1 class="text-lg"><strong>Special Attack: </strong>123</h1>
        <h1 class="text-lg"><strong>Special Defense: </strong>123</h1>
        <h1 class="text-lg"><strong>Speed: </strong>123</h1>
      </div>
    </div>

  </template>
  <template v-else>
    <h1 class="text-gray-700 text-2xl font-bold h-96 flex justify-center items-center">{{ message }}</h1> 
  </template>
</template>
