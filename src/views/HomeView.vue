<script setup>
  import { ref, reactive } from 'vue';

  const name = ref('');
  const message = ref('Digitando o nome de um pokemon para saber todos os detalhes dele');
  const pokemon = ref({});

  function searchPokemon(){
    const pokemonName = String(name.value).toLowerCase();

    pokemon.value = {}
    message.value = 'Estamos buscando o seu pokemon'

    fetch(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`)
    .then(response => response.json())
    .then(response => {
      pokemon.value = response
      message.value = 'Digitando o nome de um pokemon você para todos os detalhes dele'
    })
    .catch(error => {
      console.log(error)
      message.value = 'Não encontramos esse pokemon, tente pesquisar novamente'
    })
  }
</script>

<template>
  <main>
    <form action="" @submit.prevent="searchPokemon">
      <input type="text" name="namePokemon" placeholder="Qual pokémon você está procurando?" v-model="name">
    </form>
  </main>

  <template v-if="Object.keys(pokemon).length !== 0">
    
    <div class="mt-4">
      <h1>{{ pokemon.name }}</h1>
      <img :src="pokemon.sprites.other.home.front_default" alt="" />
    </div>

  </template>
  <template v-else>
    {{ message }}
  </template>
</template>
