<script setup>
  import { ref, reactive } from 'vue';

  const name = ref('');

  function searchPokemon(){
    const pokemonName = String(name.value).toLowerCase();

    fetch(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`)
    .then(response => response.json())
    .then(response => {
      console.log(response)
    })
    .catch(error => {
      console.log(error)
      return alert('Não encontramos esse pokémon');
    })
  }
</script>

<template>
  <main>
    <form action="" @submit.prevent="searchPokemon">
      <input type="text" name="namePokemon" placeholder="Qual pokémon você está procurando?" v-model="name">
    </form>
  </main>
</template>
